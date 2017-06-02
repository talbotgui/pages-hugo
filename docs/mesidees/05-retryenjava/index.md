---
title: Coder des ré-essais en Java
prev: /mesidees/04-consolehibernate/
next: /mesidees/06-relecturedecode/
weight: 105

---

La librairie **guava-retrying** est assez pratique pour définir des méthodes à ré-essayer en cas d'échec.

Documentation : [ici] (https://github.com/rholder/guava-retrying)

#### Dépendence
```xml
<dependency>
	<groupId>com.github.rholder</groupId>
	<artifactId>guava-retrying</artifactId>
	<version>2.0.0</version>
	<scope>test</scope>
</dependency>
```

#### Exemple
```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.TimeUnit;

import org.junit.Assert;

import com.github.rholder.retry.RetryException;
import com.github.rholder.retry.Retryer;
import com.github.rholder.retry.RetryerBuilder;
import com.github.rholder.retry.StopStrategies;
import com.github.rholder.retry.WaitStrategies;

public class Test {
	public void testRetry() throws ExecutionException, RetryException {
		final int nbEssais = 6;
		final int tempsEntreDeuxEssaisEnMs = 500;

		final Retryer<Void> retryer = RetryerBuilder.<Void>newBuilder()//
				.retryIfExceptionOfType(AssertionError.class).retryIfRuntimeException()//
				.withStopStrategy(StopStrategies.stopAfterAttempt(nbEssais))//
				.withWaitStrategy(WaitStrategies.fixedWait(tempsEntreDeuxEssaisEnMs, TimeUnit.MILLISECONDS))//
				.build();

		final Callable<Void> assertion = () -> {
			Assert.assertNotNull("attendu");
			return null;
		};

		retryer.call(assertion);
	}
}
```