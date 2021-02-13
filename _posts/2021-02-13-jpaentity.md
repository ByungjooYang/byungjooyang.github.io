---
title:  "Could not open JPA  EntityManager for transaction"
header:
  teaser: 
categories: 
  - 
tags:
  - 
---

스프링 부트 프로젝트 진행을 하던 중 

could not open jpa entitymanager for transaction;

이라는 에러가 발생했다.

빌드 도구는 그래들로 진행 중이었는데 

인텔리제이에서 실행시에는 문제가 생기지 않았지만 

ec2 서버에서 deploy 시키거나 Travis-CI 에서 빌드시 

이 문제가 발생했다.

```ruby

org.springframework.transaction.CannotCreateTransactionException: Could not open JPA  EntityManager for transaction; nested exception is java.lang.IllegalStateException: Already value [org.springframework.jdbc.datasource.ConnectionHolder@43f9e588] for key [org.springframework.jdbc.datasource.DriverManagerDataSource@84f171a] bound to thread [jobLauncherTaskExecutor-1]
     at org.springframework.orm.jpa.JpaTransactionManager.doBegin(JpaTransactionManager.java:427)
     at org.springframework.transaction.support.AbstractPlatformTransactionManager.getTransaction(AbstractPlatformTransactionManager.java:371)
     at org.springframework.transaction.interceptor.TransactionAspectSupport.createTransactionIfNecessary(TransactionAspectSupport.java:335)
     at org.springframework.transaction.interceptor.TransactionInterceptor.invoke(TransactionInterceptor.java:105)
     at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:172)
     at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:202)
     at com.sun.proxy.$Proxy41.saveIfUnique(Unknown Source)
     at com.qompa.batch.ArticleItemWriter.write(ArticleItemWriter.java:28)
     at org.springframework.batch.core.step.item.SimpleChunkProcessor.writeItems(SimpleChunkProcessor.java:171)
     at org.springframework.batch.core.step.item.SimpleChunkProcessor.doWrite(SimpleChunkProcessor.java:150)
     at org.springframework.batch.core.step.item.FaultTolerantChunkProcessor$3.doWithRetry(FaultTolerantChunkProcessor.java:313)
     at org.springframework.batch.retry.support.RetryTemplate.doExecute(RetryTemplate.java:240)
     at org.springframework.batch.retry.support.RetryTemplate.execute(RetryTemplate.java:187)
     at org.springframework.batch.core.step.item.BatchRetryTemplate.execute(BatchRetryTemplate.java:213)
     at org.springframework.batch.core.step.item.FaultTolerantChunkProcessor.write(FaultTolerantChunkProcessor.java:402)
     at org.springframework.batch.core.step.item.SimpleChunkProcessor.process(SimpleChunkProcessor.java:194)
     at org.springframework.batch.core.step.item.ChunkOrientedTasklet.execute(ChunkOrientedTasklet.java:74)
     at org.springframework.batch.core.step.tasklet.TaskletStep$ChunkTransactionCallback.doInTransaction(TaskletStep.java:386)
     at org.springframework.transaction.support.TransactionTemplate.execute(TransactionTemplate.java:130)
     at org.springframework.batch.core.step.tasklet.TaskletStep$2.doInChunkContext(TaskletStep.java:264)
     at org.springframework.batch.core.scope.context.StepContextRepeatCallback.doInIteration(StepContextRepeatCallback.java:76)
     at org.springframework.batch.repeat.support.RepeatTemplate.getNextResult(RepeatTemplate.java:367)
     at org.springframework.batch.repeat.support.RepeatTemplate.executeInternal(RepeatTemplate.java:214)
     at org.springframework.batch.repeat.support.RepeatTemplate.iterate(RepeatTemplate.java:143)
     at org.springframework.batch.core.step.tasklet.TaskletStep.doExecute(TaskletStep.java:250)
     at org.springframework.batch.core.step.AbstractStep.execute(AbstractStep.java:195)
     at org.springframework.batch.core.job.SimpleStepHandler.handleStep(SimpleStepHandler.java:135)
     at org.springframework.batch.core.job.flow.JobFlowExecutor.executeStep(JobFlowExecutor.java:61)
     at org.springframework.batch.core.job.flow.support.state.StepState.handle(StepState.java:60)
     at org.springframework.batch.core.job.flow.support.SimpleFlow.resume(SimpleFlow.java:144)
     at org.springframework.batch.core.job.flow.support.SimpleFlow.start(SimpleFlow.java:124)
     at org.springframework.batch.core.job.flow.FlowJob.doExecute(FlowJob.java:135)
     at org.springframework.batch.core.job.AbstractJob.execute(AbstractJob.java:281)
     at org.springframework.batch.core.launch.support.SimpleJobLauncher$1.run(SimpleJobLauncher.java:120)
     at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
     at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
     at java.lang.Thread.run(Thread.java:724)
Caused by: java.lang.IllegalStateException: Already value [org.springframework.jdbc.datasource.ConnectionHolder@43f9e588] for key [org.springframework.jdbc.datasource.DriverManagerDataSource@84f171a] bound to thread [jobLauncherTaskExecutor-1]
     at org.springframework.transaction.support.TransactionSynchronizationManager.bindResource(TransactionSynchronizationManager.java:189)
     at org.springframework.orm.jpa.JpaTransactionManager.doBegin(JpaTransactionManager.java:402)
... 36 more

```

아니 문제가 발생할 때도 있고 정상 빌드가 될 때도 있었다.

이런식이니 Travis-CI 의 자동 배포가 의미가 없어졌다.

빌드가 실패하면 다시 될 때까지 빌드를 시켜줘야 했기 때문이다.

며칠 동안 여러가지 찾아보고 확인을 해봐도 고쳐지지 않았다.

찾아보면 배치를 다시 해야한다고 했는데

배치는 하지도 않았는데도 이런 에러가 났다.

결국 문제를 해결했는데 확인 해보니

그래들 4버전을 사용 중 이었는데 그래들 6.7.1 버전으로 

버전을 올려주니 해결이 되었다.

6버전의 경우 dependencies에 추가할 경우

complie(), testCompile()이 아니라

implementation(), testImplementation()을 사용해야한다.

롬복의 경우 implementation() 외에도

```ruby
annotationProcessor('org.projectlombok:lombok')
testImplementation('org.projectlombok:lombok')
testAnnotationProcessor('org.projectlombok:lombok')
```

이것도 추가해 주어야 한다.

아마 이것들 때문에 문제가 발생한것 같다.
