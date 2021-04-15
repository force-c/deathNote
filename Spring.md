[TOC]

# Spring生命周期 Hook函数执行顺序

1. 构造函数
2. BeanFactoryAware.setBeanFactory()
3. ApplicationContextAware.setApplicationContext()
4. InitializingBean.afterPropertiesSet()
5. BeanFactoryPostProcessor.postProcessBeanFactory()
6. InstantiationAwareBeanPostProcessorAdapter.postProcessBeforeInstantiation()
7. InstantiationAwareBeanPostProcessorAdapter.postProcessBeforeInstantiation()
8. InstantiationAwareBeanPostProcessorAdapter.postProcessAfterInstantiation()
9. BeanPostProcessor.postProcessBeforeInitialization()
10. BeanPostProcessor.postProcessAfterInitialization()
11. InstantiationAwareBeanPostProcessorAdapter.postProcessAfterInstantiation()
12. BeanPostProcessor.postProcessBeforeInitialization()
13. BeanPostProcessor.postProcessAfterInitialization()

# 获取Spring容器种所有的BeanName

```java
// 实现 interface ApplicationContextAware

/**
 * @author guochuang
 * @version 1.0
 * @date 2021/3/9 13:16
 */
@Component
public class ApplicationContextBean implements ApplicationContextAware, InitializingBean {
    private static final Logger LOGGER = LoggerFactory.getLogger(ApplicationContextBean.class);
    public static ApplicationContext applicationContext;
    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        ApplicationContextBean.applicationContext = applicationContext;
    }


    @Override
    public void afterPropertiesSet() throws Exception {
        int beanDefinitionCount = applicationContext.getBeanDefinitionCount();
        String[] beanDefinitionNames = applicationContext.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            if (beanDefinitionName.contains(".")) {
                LOGGER.info(beanDefinitionName.substring(beanDefinitionName.lastIndexOf(".") + 1));

            } else {
                LOGGER.info(beanDefinitionName);
            }
        }
        LOGGER.info("count -> {}", beanDefinitionCount);
    }
}
```



# spring事务

## spring事务传播机制

```sh
# 必要事务
# PROPAGATION_REQUIRED

# 支持事务
# PROPAGATION_SUPPORTS

# 强制事务
# PROPAGATION_MANDATORY

# 创建新事务
# PROPAGATION_REQUIRES_NEW

# 不支持事务
# PROPAGATION_NOT_SUPPORTED

# 非事务
# PROPAGATION_NEVER

# 嵌套事务
# PROPAGATION_NESTED
```

## spring事务 隔离级别

```sh
# 数据库默认隔离级别
# ISOLATION_DEFAULT

# 读未提交
# ISOLATION_READ_UNCOMMITTED
# 允许另外一个事务可以看到这个事务未提交的数据

# 读已提交
# ISOLATION_READ_COMMITTED
# 保证一个事务修改的数据提交后才能被另一事务读取，而且能看到该事务对已有记录的更新

# 可重复读
# ISOLATION_REPEATABLE_READ
# 保证一个事务修改的数据提交后才能被另一事务读取，但是不能看到该事务对已有记录的更新

# 序列化
# ISOLATION_SERIALIZABLE
# 一个事务在执行的过程中完全看不到其他事务对数据库所做的更新
```





