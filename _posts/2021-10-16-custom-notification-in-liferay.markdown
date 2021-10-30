---
layout: post
title: "Custom Notification In Liferay"
date: 2021-10-23 16:39:00 +0530
categories: liferay
author: hardik
thumbnail: /assets/images/post/noti_in_liferay.png
comments_id: 1
---

In this post, I am going to talk about notification feature and how to create custom notification in Liferay.
<!--more-->
Let's start customization...

## 1. Create module in Liferay workspace
- To create Liferay modules.
- File menu &rarr; Click New &rarr; Select Liferay Module Project. 
- Provide appropriate name for module and click finish.

## 2. Create 'SendNotificationToUserHandler' Class
- Create SendNotificationToUserHandler class and extends **BaseUserNotificationHandler** abstract class.

``` java
import org.osgi.service.component.annotations.Component;

import com.liferay.portal.kernel.json.JSONFactoryUtil;
import com.liferay.portal.kernel.json.JSONObject;
import com.liferay.portal.kernel.model.UserNotificationEvent;
import com.liferay.portal.kernel.notifications.BaseUserNotificationHandler;
import com.liferay.portal.kernel.notifications.UserNotificationHandler;
import com.liferay.portal.kernel.service.ServiceContext;
import com.liferay.portal.kernel.util.StringUtil;

@Component(service = UserNotificationHandler.class)
public class SendNotificationToUserHandler extends BaseUserNotificationHandler{
	public static final String PORLET_ID = "com.xyz.news.items";
	
	public SendNotificationToUserHandler() {
		setPortletId(SendNotificationToUserHandler.PORLET_ID);
	}

	@Override
	protected String getBody(UserNotificationEvent userNotificationEvent, ServiceContext serviceContext)
			throws Exception {
		JSONObject jsonObject = JSONFactoryUtil.createJSONObject(userNotificationEvent.getPayload());
		String notificationTitle=jsonObject.getString("title");
		String notificationDetails = jsonObject.getString("status");
		String body = StringUtil.replace(getBodyTemplate(), 
				new String[] {"[$TITLE$]", "[$BODY_TEXT$]" },
				new String[] {"<strong>" + notificationTitle + "</strong>", notificationDetails });
		return body;
	}
	
	@Override
	protected String getLink(
		UserNotificationEvent userNotificationEvent,
		ServiceContext serviceContext) throws Exception {
		return super.getLink(userNotificationEvent, serviceContext);
	}

	@Override
	protected String getBodyTemplate() throws Exception {
		return "<div class=\"title\">[$TITLE$]</div><div>[$BODY_TEXT$]</div>";
	}
}
```

-  Above code used for notification format.

## 3. Create one method for save entry in database
- Create one method to save notification data  in database.
- It's depend on project structure where to create this method.

* 'payload' object for notification data like below.

``` java
JSONObject payload = JSONFactoryUtil.createJSONObject();
payload.put("status", "This text for notification description in brief." );
payload.put("title", "Notification Title");
payload.put("userId", "userId who send this notification");
```

* 'User' is Liferay's User Object

``` java
private void sendNotification(User user, JSONObject payload) throws PortalException {
		SendNotificationToUserHandler hndlr = new SendNotificationToUserHandler();
		UserNotificationManagerUtil.addUserNotificationHandler(hndlr);
		
		if(UserNotificationManagerUtil.fetchUserNotificationDefinition(
				hndlr.getPortletId(),
				ClassNameLocalServiceUtil.getClassNameId(hndlr.getClass().getName()),
				UserNotificationDeliveryConstants.TYPE_WEBSITE) == null)
			{

			    UserNotificationDefinition def =
					new UserNotificationDefinition(
						hndlr.getPortletId(),
						ClassNameLocalServiceUtil.getClassNameId(
							hndlr.getClass().getName()
						),
						0,
						"Receive notification when this " +
						"portlet does something INCREDIBLE." );

			    def.addUserNotificationDeliveryType(
					new UserNotificationDeliveryType(
				    	"Website",
					    UserNotificationDeliveryConstants.TYPE_WEBSITE,
						true,
						true)
				);

				UserNotificationManagerUtil.addUserNotificationDefinition(
					hndlr.getPortletId(),
					def);
			}
		

		UserNotificationEvent notification =
			UserNotificationEventLocalServiceUtil.createUserNotificationEvent(
				CounterLocalServiceUtil.increment()
			);

		notification.setCompanyId(user.getCompanyId());
		notification.setUserId(user.getUserId());
		notification.setPayload(payload.toString());
		notification.setDeliveryType(UserNotificationDeliveryConstants.TYPE_WEBSITE);
		notification.setTimestamp(new Date().getTime());
		notification.setArchived(false);
		notification.setDelivered(true);
		notification.setType(hndlr.getPortletId());

		UserNotificationEventLocalServiceUtil.addUserNotificationEvent(notification);
	}
```