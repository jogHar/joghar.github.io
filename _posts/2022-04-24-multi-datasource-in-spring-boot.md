---
layout: post
title: Mutli datasource in spring boot
date: 2022-04-24 14:00:00 +0530
categories: spring-boot
author: hardik
thumbnail: /assets/images/post/boot-multi-datasource.jpg
comments_id: 3
---

Spring boot is known for auto-configuration and create stand-alone application. Spring boot with JPA is easy to connect database without create entity manager and transaction manager, Here we talk to how to connect multi-datasource with spring boot application.
<!--more-->

Multi datasource connection with spring boot is not rocket science. We need to just create some basic configuration for it. let's start...

First of all we add to property in properties file like below.
Here I'm choose one database is postgres and second is MySQL.
You can connect more then two database.
```
# application.properties

# Primary datasource
db1.datasource.url=jdbc:postgresql://localhost:5432/xyz?autoReconnect=true&characterEncoding=UTF-8
db1.datasource.username=username
db1.datasource.password=password
db1.datasource.configuration.maximum-pool-size=30
db1.datasource.configuration.connection-timeout= 15000
db1.hibernate.dialect=org.hibernate.spatial.dialect.postgis.PostgisDialect
db1.hibernate.hbm2ddl.auto=none

# Secondary datasource
db2.datasource.url=jdbc:mysql://localhost:3306/xyz?autoReconnect=true&characterEncoding=UTF-8
db2.datasource.username=username
db2.datasource.password=password
db2.datasource.configuration.maximum-pool-size=30
db2.datasource.configuration.connection-timeout= 15000
db2.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
db2.hibernate.hbm2ddl.auto=none
```

Create one package for Datasource configuration, not required is up to you
- Primary data source configuration file

```java
// PrimaryDataSourceConfiguration.java

import java.util.HashMap;
import java.util.Map;

import javax.persistence.EntityManagerFactory;
import javax.sql.DataSource;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.autoconfigure.jdbc.DataSourceProperties;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.orm.jpa.EntityManagerFactoryBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.core.env.Environment;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;
import org.springframework.orm.jpa.JpaTransactionManager;
import org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import com.zaxxer.hikari.HikariDataSource;

@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(entityManagerFactoryRef = "primaryEntityManagerFactory", transactionManagerRef = "primaryTransactionManager", basePackages = {
		"com.xyz.primary.repository" })
public class PrimaryDataSourceConfiguration {
	@Autowired
	private Environment env;

	@Primary
	@Bean(name = "primaryDataSourceProperties")
	@ConfigurationProperties("db1.datasource")
	public DataSourceProperties primaryDataSourceProperties() {
		return new DataSourceProperties();
	}

	@Primary
	@Bean(name = "primaryDataSource")
	@ConfigurationProperties("db1.datasource.configuration")
	public DataSource primaryDataSource(
			@Qualifier("primaryDataSourceProperties") DataSourceProperties primaryDataSourceProperties) {
		return primaryDataSourceProperties.initializeDataSourceBuilder().type(HikariDataSource.class).build();
	}

	@Primary
	@Bean(name = "primaryEntityManagerFactory")
	public LocalContainerEntityManagerFactoryBean primaryEntityManagerFactory(
			EntityManagerFactoryBuilder primaryEntityManagerFactoryBuilder,
			@Qualifier("primaryDataSource") DataSource primaryDataSource) {

		Map<String, String> primaryJpaProperties = new HashMap<>();
		primaryJpaProperties.put("hibernate.dialect", env.getProperty("db1.hibernate.dialect"));
		primaryJpaProperties.put("hibernate.hbm2ddl.auto", env.getProperty("db1.hibernate.hbm2ddl.auto"));

		return primaryEntityManagerFactoryBuilder.dataSource(primaryDataSource)
				.packages("com.xyz.primary.entity")
				.persistenceUnit("primaryDataSource").properties(primaryJpaProperties).build();
	}

	@Primary
	@Bean(name = "primaryTransactionManager")
	public PlatformTransactionManager primaryTransactionManager(
			@Qualifier("primaryEntityManagerFactory") EntityManagerFactory primaryEntityManagerFactory) {
		return new JpaTransactionManager(primaryEntityManagerFactory);
	}
}
```

- Secondary data-source configuration

```java
// SecondaryDataSourceConfiguration.java

package com.xyz.datasource.config;

import java.util.HashMap;
import java.util.Map;

import javax.persistence.EntityManagerFactory;
import javax.sql.DataSource;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.autoconfigure.jdbc.DataSourceProperties;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.orm.jpa.EntityManagerFactoryBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.env.Environment;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;
import org.springframework.orm.jpa.JpaTransactionManager;
import org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import com.zaxxer.hikari.HikariDataSource;

@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(entityManagerFactoryRef = "secondaryEntityManagerFactory", transactionManagerRef = "secondaryTransactionManager", basePackages = {
		"com.xyz.secondary.repository" })
public class SecondaryDataSourceConfiguration {
	@Autowired
	private Environment env;

	@Bean(name = "secondaryDataSourceProperties")
	@ConfigurationProperties("db2.datasource")
	public DataSourceProperties secondaryDataSourceProperties() {
		return new DataSourceProperties();
	}

	@Bean(name = "secondaryDataSource")
	@ConfigurationProperties("db2.datasource.configuration")
	public DataSource secondaryDataSource(
			@Qualifier("secondaryDataSourceProperties") DataSourceProperties secondaryDataSourceProperties) {
		return secondaryDataSourceProperties.initializeDataSourceBuilder().type(HikariDataSource.class).build();
	}

	@Bean(name = "secondaryEntityManagerFactory")
	public LocalContainerEntityManagerFactoryBean secondaryEntityManagerFactory(
			EntityManagerFactoryBuilder secondaryEntityManagerFactoryBuilder,
			@Qualifier("secondaryDataSource") DataSource secondaryDataSource) {

		Map<String, String> secondaryJpaProperties = new HashMap<>();
		secondaryJpaProperties.put("hibernate.dialect", env.getProperty("db2.hibernate.dialect"));
		secondaryJpaProperties.put("hibernate.hbm2ddl.auto", env.getProperty("db2.hibernate.hbm2ddl.auto"));

		return secondaryEntityManagerFactoryBuilder.dataSource(secondaryDataSource)
				.packages("com.xyz.secondary.entity").persistenceUnit("secondaryDataSource")
				.properties(secondaryJpaProperties).build();
	}

	@Bean(name = "secondaryTransactionManager")
	public PlatformTransactionManager secondaryTransactionManager(
			@Qualifier("secondaryEntityManagerFactory") EntityManagerFactory secondaryEntityManagerFactory) {
		return new JpaTransactionManager(secondaryEntityManagerFactory);
	}
}
```

Above configuration is scanning 4 packages for entity and repository, so create as above data-source configuration.

- com.xyz.primary.entity
- com.xyz.primary.repository
- com.xyz.secondary.entity
- com.xyz.secondary.repository

