# HBConfig
HBConfig
package com.book.mvc.config;

import java.util.Properties;

import javax.sql.DataSource;

  import org.hibernate.SessionFactory;
  import org.hibernate.cfg.AvailableSettings;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.ComponentScan;
  import org.springframework.context.annotation.Configuration;
  import org.springframework.context.annotation.PropertySource;
  import org.springframework.core.env.Environment;
  import org.springframework.jdbc.datasource.DriverManagerDataSource;
  import org.springframework.orm.hibernate5.HibernateTransactionManager;
  import org.springframework.orm.hibernate5.LocalSessionFactoryBean;
  import org.springframework.stereotype.Component;
  import org.springframework.transaction.annotation.EnableTransactionManagement;

  @Configuration
  @ComponentScan({ "com.book.mvc.config" })
  @EnableTransactionManagement
 //@PropertySource("classpath:hibernate.properties")
  public class HibernateConfig {
	
	//@Autowired
	//private Environment env;
	
	
	@Bean
	public DataSource getDataSource() {
		DriverManagerDataSource dataSource = new DriverManagerDataSource();
		dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
		dataSource.setUrl("jdbc:mysql://");
		dataSource.setUsername("username");
		dataSource.setPassword("password");
		return dataSource;
	}
	
	@Bean
	public LocalSessionFactoryBean SessionFactory() {
		LocalSessionFactoryBean sessionFactory = new LocalSessionFactoryBean();
		sessionFactory.setDataSource(getDataSource());
		sessionFactory.setPackagesToScan(new String[] { "com.book.mvc.model" });
		sessionFactory.setHibernateProperties(getHibernateProperties());
		return sessionFactory;
	}

	private Properties getHibernateProperties() {
		Properties properties = new Properties();
		properties.put(AvailableSettings.DIALECT, "org.hibernate.dialect.MySQL5Dialect");
		properties.put(AvailableSettings.SHOW_SQL, true);
		//properties.put(AvailableSettings.STATEMENT_BATCH_SIZE, env.getRequiredProperty("hibernate.batch.size"));
		properties.put(AvailableSettings.HBM2DDL_AUTO, "update");
		//properties.put(AvailableSettings.CURRENT_SESSION_CONTEXT_CLASS, env.getRequiredProperty("hibernate.current.session.context.class"));
		//properties.put(AvailableSettings.C3P0_MAX_SIZE, env.getRequiredProperty("hibernate.c3p0.max_size"));
		//properties.put(AvailableSettings.C3P0_MIN_SIZE, env.getRequiredProperty("hibernate.c3p0.min_size"));
		//properties.put(AvailableSettings.C3P0_MAX_STATEMENTS, env.getRequiredProperty("hibernate.c3p0.max_statements"));
		//properties.put(AvailableSettings.C3P0_TIMEOUT, env.getRequiredProperty("hibernate.c3p0.timeout"));
		return properties;
	}

	@Bean
	public HibernateTransactionManager transactionManager(SessionFactory sessionFactory) {
		HibernateTransactionManager txManager = new HibernateTransactionManager();
		txManager.setSessionFactory(SessionFactory().getObject());
		return txManager;
	}
	
}
