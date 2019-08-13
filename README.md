# mule-domain-issue-example


## Reproduction Steps

```sh
$ echo $MULE_HOME
/opt/mule/mule-enterprise-standalone

# Deploy the domain for the first time
$ mv mule-domain-issue-example-1.0.0-SNAPSHOT-mule-domain.jar $MULE_HOME/domains/

# Check domains folder
$ ll $MULE_HOME/domains | grep mule-domain-issue-example
drwxr-xr-x. 4 mule mule 70 Aug 12 16:37 mule-domain-issue-example-1.0.0-SNAPSHOT-mule-domain
-rw-r--r--. 1 mule mule 77 Aug 12 16:37 mule-domain-issue-example-1.0.0-SNAPSHOT-mule-domain-anchor.txt

# Check mule_ee.log
$ tail $MULE_HOME/logs/mule_ee.log
**********************************************************************
* Started domain                                                     *
* 'mule-domain-issue-example-1.0.0-SNAPSHOT-mule-domain'             *
* Domain plugins:                                                    *
*  - ObjectStore : 1.1.3                                             *
**********************************************************************

# Deploy the same domain file again
$ mv mule-domain-issue-example-1.0.0-SNAPSHOT-mule-domain.jar $MULE_HOME/domains/

# Check domains folder
# NOTE: No anchor file created
$ ll $MULE_HOME/domains | grep example
drwxr-xr-x. 4 mule mule 70 Aug 12 17:01 mule-domain-issue-example-1.0.0-SNAPSHOT-mule-domain

$ cd $MULE_HOME/logs
$ cat mule-domain-mule-domain-issue-example-1.0.0-SNAPSHOT-mule-domain.log

WARN  2019-08-12 17:12:22,058 [Mule.app.deployer.monitor.1.thread.1] [event: ] org.mule.runtime.config.internal.MuleArtifactContext: Exception encountered during context initialization - cancelling refresh attempt: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'Domain_Object_store': FactoryBean threw exception on object creation; nested exception is org.mule.runtime.api.exception.MuleRuntimeException: org.mule.runtime.api.lifecycle.LifecycleException: Error creating bean with name 'org.mule.extension.objectstore.api.TopLevelObjectStore': Unsatisfied dependency expressed through field 'registry'; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'org.mule.extension.objectstore.internal.ObjectStoreRegistry' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@javax.inject.Inject()}
ERROR 2019-08-12 17:12:22,151 [Mule.app.deployer.monitor.1.thread.1] [event: ] org.mule.runtime.module.deployment.impl.internal.domain.DefaultMuleDomain: Error creating bean with name 'Domain_Object_store': FactoryBean threw exception on object creation; nested exception is org.mule.runtime.api.exception.MuleRuntimeException: org.mule.runtime.api.lifecycle.LifecycleException: Error creating bean with name 'org.mule.extension.objectstore.api.TopLevelObjectStore': Unsatisfied dependency expressed through field 'registry'; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'org.mule.extension.objectstore.internal.ObjectStoreRegistry' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@javax.inject.Inject()}
org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'org.mule.extension.objectstore.internal.ObjectStoreRegistry' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@javax.inject.Inject()}
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.raiseNoMatchingBeanFound(DefaultListableBeanFactory.java:1654) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1213) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1167) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:593) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:90) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessProperties(AutowiredAnnotationBeanPostProcessor.java:374) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1411) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.autowireBean(AbstractAutowireCapableBeanFactory.java:316) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.mule.runtime.config.internal.SpringRegistry.initialiseObject(SpringRegistry.java:315) ~[mule-module-spring-config-4.2.1.jar:4.2.1]
	at org.mule.runtime.config.internal.SpringRegistry.inject(SpringRegistry.java:305) ~[mule-module-spring-config-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.extension.internal.config.dsl.parameter.TopLevelParameterObjectFactory.lambda$doGetObject$3(TopLevelParameterObjectFactory.java:80) ~[mule-module-extensions-spring-support-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.api.util.ExceptionUtils.tryExpecting(ExceptionUtils.java:227) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.api.util.ClassUtils.withContextClassLoader(ClassUtils.java:915) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.extension.internal.config.dsl.parameter.TopLevelParameterObjectFactory.doGetObject(TopLevelParameterObjectFactory.java:62) ~[mule-module-extensions-spring-support-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.extension.internal.config.dsl.parameter.TopLevelParameterObjectFactory$$EnhancerByCGLIB$$56b31a01.CGLIB$doGetObject$1(<generated>) ~[mule-module-extensions-spring-support-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.extension.internal.config.dsl.parameter.TopLevelParameterObjectFactory$$EnhancerByCGLIB$$56b31a01$$FastClassByCGLIB$$c124a14f.invoke(<generated>) ~[mule-module-extensions-spring-support-4.2.1.jar:4.2.1]
	at net.sf.cglib.proxy.MethodProxy.invokeSuper(MethodProxy.java:228) ~[cglib-nodep-3.2.10.jar:?]
	at org.mule.runtime.config.internal.dsl.spring.ObjectFactoryClassRepository.lambda$getObjectFactoryDynamicClass$1(ObjectFactoryClassRepository.java:114) ~[mule-module-spring-config-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.extension.internal.config.dsl.parameter.TopLevelParameterObjectFactory$$EnhancerByCGLIB$$56b31a01.doGetObject(<generated>) ~[mule-module-extensions-spring-support-4.2.1.jar:4.2.1]
	at org.mule.runtime.dsl.api.component.AbstractComponentFactory.getObject(AbstractComponentFactory.java:30) ~[mule-module-dsl-api-1.2.0.jar:?]
	at org.mule.runtime.module.extension.internal.config.dsl.AbstractExtensionObjectFactory.getObject(AbstractExtensionObjectFactory.java:68) ~[mule-module-extensions-spring-support-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.extension.internal.config.dsl.parameter.TopLevelParameterObjectFactory$$EnhancerByCGLIB$$56b31a01.CGLIB$getObject$8(<generated>) ~[mule-module-extensions-spring-support-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.extension.internal.config.dsl.parameter.TopLevelParameterObjectFactory$$EnhancerByCGLIB$$56b31a01$$FastClassByCGLIB$$c124a14f.invoke(<generated>) ~[mule-module-extensions-spring-support-4.2.1.jar:4.2.1]
	at net.sf.cglib.proxy.MethodProxy.invokeSuper(MethodProxy.java:228) ~[cglib-nodep-3.2.10.jar:?]
	at org.mule.runtime.config.internal.dsl.spring.ObjectFactoryClassRepository.lambda$getObjectFactoryDynamicClass$1(ObjectFactoryClassRepository.java:104) ~[mule-module-spring-config-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.extension.internal.config.dsl.parameter.TopLevelParameterObjectFactory$$EnhancerByCGLIB$$56b31a01.getObject(<generated>) ~[mule-module-extensions-spring-support-4.2.1.jar:4.2.1]
	at org.springframework.beans.factory.support.FactoryBeanRegistrySupport.doGetObjectFromFactoryBean(FactoryBeanRegistrySupport.java:171) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.beans.factory.support.FactoryBeanRegistrySupport.getObjectFromFactoryBean(FactoryBeanRegistrySupport.java:101) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getObjectForBeanInstance(AbstractBeanFactory.java:1674) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.getObjectForBeanInstance(AbstractAutowireCapableBeanFactory.java:1249) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:257) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.mule.runtime.config.internal.ObjectProviderAwareBeanFactory.getBean(ObjectProviderAwareBeanFactory.java:73) ~[mule-module-spring-config-4.2.1.jar:4.2.1]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:844) ~[spring-beans-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:877) ~[spring-context-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:549) ~[spring-context-5.1.6.RELEASE.jar:5.1.6.RELEASE]
	at org.mule.runtime.config.internal.SpringRegistry.doInitialise(SpringRegistry.java:100) ~[mule-module-spring-config-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.internal.registry.AbstractRegistry.initialise(AbstractRegistry.java:94) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.internal.registry.MuleRegistryHelper.fireLifecycle(MuleRegistryHelper.java:111) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.internal.lifecycle.MuleContextLifecycleManager$MuleContextLifecycleCallback.onTransition(MuleContextLifecycleManager.java:73) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.internal.lifecycle.MuleContextLifecycleManager$MuleContextLifecycleCallback.onTransition(MuleContextLifecycleManager.java:69) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.privileged.lifecycle.AbstractLifecycleManager.invokePhase(AbstractLifecycleManager.java:134) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.internal.lifecycle.MuleContextLifecycleManager.fireLifecycle(MuleContextLifecycleManager.java:61) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.internal.context.DefaultMuleContext.initialise(DefaultMuleContext.java:299) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.api.context.DefaultMuleContextFactory.doCreateMuleContext(DefaultMuleContextFactory.java:188) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.api.context.DefaultMuleContextFactory.createMuleContext(DefaultMuleContextFactory.java:59) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.impl.internal.artifact.ArtifactContextBuilder.lambda$build$2(ArtifactContextBuilder.java:487) ~[mule-module-deployment-model-impl-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.api.util.ExceptionUtils.tryExpecting(ExceptionUtils.java:227) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.api.util.ClassUtils.withContextClassLoader(ClassUtils.java:915) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.api.util.ClassUtils.withContextClassLoader(ClassUtils.java:879) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.impl.internal.artifact.ArtifactContextBuilder.build(ArtifactContextBuilder.java:398) ~[mule-module-deployment-model-impl-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.impl.internal.domain.DefaultMuleDomain.doInit(DefaultMuleDomain.java:191) ~[mule-module-deployment-model-impl-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.impl.internal.domain.DefaultMuleDomain.init(DefaultMuleDomain.java:147) ~[mule-module-deployment-model-impl-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.api.util.ClassUtils.lambda$withContextClassLoader$9(ClassUtils.java:860) ~[mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.api.util.ExceptionUtils.tryExpecting(ExceptionUtils.java:227) [mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.api.util.ClassUtils.withContextClassLoader(ClassUtils.java:915) [mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.api.util.ClassUtils.withContextClassLoader(ClassUtils.java:879) [mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.core.api.util.ClassUtils.withContextClassLoader(ClassUtils.java:859) [mule-core-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.impl.internal.artifact.DeployableArtifactWrapper.executeWithinArtifactClassLoader(DeployableArtifactWrapper.java:140) [mule-module-deployment-model-impl-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.impl.internal.artifact.DeployableArtifactWrapper.init(DeployableArtifactWrapper.java:83) [mule-module-deployment-model-impl-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.internal.DefaultArtifactDeployer.doInit(DefaultArtifactDeployer.java:63) [mule-module-deployment-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.internal.DefaultArtifactDeployer.deploy(DefaultArtifactDeployer.java:28) [mule-module-deployment-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.internal.DefaultArchiveDeployer.deployArtifact(DefaultArchiveDeployer.java:475) [mule-module-deployment-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.internal.DefaultArchiveDeployer.deployPackagedArtifact(DefaultArchiveDeployer.java:527) [mule-module-deployment-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.internal.DefaultArchiveDeployer.deployPackagedArtifact(DefaultArchiveDeployer.java:226) [mule-module-deployment-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.internal.DefaultArchiveDeployer.deployPackagedArtifact(DefaultArchiveDeployer.java:411) [mule-module-deployment-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.internal.DefaultArchiveDeployer.deployPackagedArtifact(DefaultArchiveDeployer.java:56) [mule-module-deployment-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.internal.DomainArchiveDeployer.deployPackagedArtifact(DomainArchiveDeployer.java:113) [mule-module-deployment-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.internal.DomainArchiveDeployer.deployPackagedArtifact(DomainArchiveDeployer.java:33) [mule-module-deployment-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.internal.DeploymentDirectoryWatcher.deployPackedDomains(DeploymentDirectoryWatcher.java:426) [mule-module-deployment-4.2.1.jar:4.2.1]
	at org.mule.runtime.module.deployment.internal.DeploymentDirectoryWatcher.run(DeploymentDirectoryWatcher.java:296) [mule-module-deployment-4.2.1.jar:4.2.1]
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511) [?:1.8.0_212]
	at java.util.concurrent.FutureTask.runAndReset(FutureTask.java:308) [?:1.8.0_212]
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.access$301(ScheduledThreadPoolExecutor.java:180) [?:1.8.0_212]
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:294) [?:1.8.0_212]
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149) [?:1.8.0_212]
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624) [?:1.8.0_212]
	at java.lang.Thread.run(Thread.java:748) [?:1.8.0_212]

```


