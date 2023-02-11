<meta charset="utf-8">

# Client Support Sample

🤓 Prepared by: [Liliana Monsalve](https://www.linkedin.com/in/liliana-monsalve) &emsp; \| &emsp; *limonsa@gmail.com*

---

<br />

## Scenario A

**_Hello! I am trying to use _Company_ on Github Actions. Whenever I try to rerun a failed build, it runs not tests at all. Can you please help?_**

<br />Most chances you're reusing a CI Build ID for a run that was already completed. To create a new run, please use a new, unique CI Build ID. For GitHub Actions, please check the `Company.yml` file in your repository to ensure that you are including the `run_attempt` environment variable for composing a valid CI Build ID:

```
--ci-build-id "${{ github.repository }}-${{ github.run_id }}-${{ github.run_attempt}}"
```

> You can find more information [here](https://docs.cypress.io/guides/guides/parallelization#CI-Build-ID-environment-variables-by-provider)

<br />

## Scenario B

**_Video description that can help a customer to interpret the data._**

<br /> The 14-day test performance breakdown video shows the following interesting insights:

### Comparison with the previous period

* **Improved test duration:** Jan 17 - Jan 30 period showed an improved version of the test suite as the average execution duration decreased in 8.51% compared to Jan 03 - Jan 16.
* **Decreased flaky tests:** Jan 17 - Jan 30 period showed 4% less flakiness rate compared to Jan 03 -Jan 16. It is possible that the improvement in the flakiness had generated a slight increase of 3% in the failure rate, but this could be an improvement opportunity since it is possible that in the previous period the failed tests were unnoticeable being disguised as flaky tests.

### Day-by-day analysis remarks

* **Best performance:** Jan 21 had no execution failed tests and no flaky tests; It is important to mention that on that day the smaller number of executed tests over the analyzed period.
* **Worst performance:** Jan 19 showed 57% failure rate not affected by flaky tests since the flakiness rate was only 3%.
* **Augmented failure time:** Jan 23 presented 90% tests executions passed and 10% flaky test resulting in a peak in the failed duration time probably caused by the flaky tests. A similar situation happened on Jan 26 when only 3% of the test failed but having 8% of flaky tests created a peak on the failed duration time.

**Recommendation:** The most encountered error was `Network Error`. It might be useful to validate with your development team the use of proper `stubs` to mock the connections to the APIs to avoid Network Errors.

<br />

## Scenario C

**_Helping a customer creating a custom integration with _Company_. The customer wants to generate a report with the details of all the failed tests on their own servers._**

<br />To obtain the details of all the failed tests on your own servers, you can trigger a HTTP POST request by enabling HTTP Webhooks and implementing a proper endpoint on your side to gather all the data needed to build your report. _Company_ will send a POST payload with a JSON-encoded response to your webhook's configured URL endpoint with the following schema:

```
{
    event: "RUN_START" | "RUN_FINISH" | "RUN_TIMEOUT" | "RUN_CANCELED";
    runUrl: string; // Company dashoard run URL
    buildId: string; // as reported by CI
    groupId: string; // only for multigroup runs
    tags: string[];
    commit: {
        sha: string | null;
        branch: string | null;
        authorName: string | null;
        authorEmail: string | null;
        message: string | null;
        remoteOrigin: string | null;
        defaultBranch: string | null;
    },
    overall: number; // overall number of tests
    passes: number; // number of passed tests
    failures: number; // number of failed tests
    pending: number; // number of pending tests
    skipped: number; // number of skipped tests
    retries: number; // number of test retries for the run
    flaky: number; // number of flaky tests for the run
}
```

<br />

## Scenario D

**_Hello! I am using a custom script to run your Company service. Here’s my script:_**

```
import { patch } from '@Company/cli';
import { run } from 'cypress';

async function main() {
    await patch();
    return run
        .run({
        parallel: true,
        record: true,
        key: "xxx",
        ciBuildId: new Date().toISOString(),
    })
    .then((results) => {
        console.log(results.runs);
    })
    .catch((err) => {
        console.error(err);
    });
}

main().catch((error) => {
    console.error(error);
    process.exit(1);
});
```

**_After upgrading to cypress 12.3.0 I get the following error:  `Integrity check failed` 
What should I do? How should the script look like?_**

<br />Cypress.io team has released a series of breaking changes in version 12+ - it triggers Integrity check failed error when using certain versions of _Company_. Please make sure to use the latest compatible version. In your case:

|  Cypress 	  | @_Company_/cli    |
|:-----------:|------------------|
|   12.3.0	   | 4.0.2+           |
> You can find more information [here](https://docs.cypress.io/guides/references/changelog#12-3-0)

<br />Please also notice that _Company_ Version 4+ doesn't modify the local installation of Cypress. The following complimentary binaries were deprecated:
* `Company-prepare` script is deprecated. Use `run` or `spawn` API instead. 
* `Company-reset` script is deprecated, use `run` or `spawn` API instead. 
* `patch` API is deprecated. Use `run` or `spawn` instead.

Therefore, we recommend you to import `run` or `spawn` instead of `patch` in the first line of your script.

<br />

---

&copy; [Liliana Monsalve](https://www.linkedin.com/in/liliana-monsalve) &emsp; \| &emsp; *limonsa@gmail.com*


