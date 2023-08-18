# testng-browserstack

[TestNG](http://testng.org) Integration with BrowserStack.

![BrowserStack Logo](https://d98b8t1nnulk5.cloudfront.net/production/images/layout/logo-header.png?1469004780)

## Using Maven

### Run sample build

- Clone the repository
- Replace YOUR_USERNAME and YOUR_ACCESS_KEY with your BrowserStack access credentials in browserstack.yml.
- Install dependencies `mvn compile`
- To run the test suite having cross-platform with parallelization, run `mvn test -P sample-test`
- To run local tests, run `mvn test -P sample-local-test`

Understand how many parallel sessions you need by using our [Parallel Test Calculator](https://www.browserstack.com/automate/parallel-calculator?ref=github)

### Integrate your test suite

This repository uses the BrowserStack SDK to run tests on BrowserStack. Follow the steps below to install the SDK in your test suite and run tests on BrowserStack:

* Create sample browserstack.yml file with the browserstack related capabilities with your [BrowserStack Username and Access Key](https://www.browserstack.com/accounts/settings) and place it in your root folder.
* Add maven dependency of browserstack-java-sdk in your pom.xml file
```sh
<dependency>
    <groupId>com.browserstack</groupId>
    <artifactId>browserstack-java-sdk</artifactId>
    <version>LATEST</version>
    <scope>compile</scope>
</dependency>
```
* Modify your build plugin to run tests by adding argLine `-javaagent:${com.browserstack:browserstack-java-sdk:jar}` and `maven-dependency-plugin` for resolving dependencies in the profiles `sample-test` and `sample-local-test`.
```
            <plugin>
               <artifactId>maven-dependency-plugin</artifactId>
                 <executions>
                   <execution>
                     <id>getClasspathFilenames</id>
                       <goals>
                         <goal>properties</goal>
                       </goals>
                   </execution>
                 </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.0.0-M5</version>
                <configuration>
                    <suiteXmlFiles>
                        <suiteXmlFile>config/sample-local-test.testng.xml</suiteXmlFile>
                    </suiteXmlFiles>
                    <argLine>
                        -javaagent:${com.browserstack:browserstack-java-sdk:jar}
                    </argLine>
                </configuration>
            </plugin>
```
* Install dependencies `mvn compile`

## Running Your Tests

### Run a specific test on BrowserStack

In this section, we will run a single test on Chrome browser on Browserstack. To change test capabilities for this configuration, please refer to the [`browserstack-single.yml`](src/test/resources/conf/browserstack-single.yml) file.

- How to run the test?

  - To run the default test scenario (e.g. End to End Scenario) on your own machine, use the following command:

  Maven:

  ```sh
  mvn test -P bstack-single -Dbrowserstack.config="./src/test/resources/conf/browserstack-single.yml"
  ```

  Gradle:

  ```sh
  gradle clean bstack-single -Dbrowserstack.config="./src/test/resources/conf/browserstack-single.yml"
  ```

  To run a specific test scenario, use the following command with the additional 'test-name' argument:

  Maven:
  ```sh
  mvn test -P bstack-single -Dbrowserstack.config="./src/test/resources/conf/browserstack-single.yml" -Dtest-name="<Test scenario name>"
  ```

  Gradle:
  ```sh
  gradle clean bstack-single -Dbrowserstack.config="./src/test/resources/conf/browserstack-single.yml" -Dtest-name="<Test scenario name>"
  ```

  where,  the argument 'test-name' can be any scenario name configured in this repository.


- Output

  This run profile executes a single test on a single browser on BrowserStack. Please refer to your [BrowserStack dashboard](https://automate.browserstack.com/) for test results.


### Run the entire test suite in parallel on a single BrowserStack browser

In this section, we will run the tests in parallel on a single browser on Browserstack. Refer to [`browserstack-parallel.yml`](src/test/resources/conf/browserstack-parallel.yml) file to change test capabilities for this configuration.

- How to run the test?

  To run the entire test suite in parallel on a single BrowserStack browser, use the following command:

  Maven:

  ```sh
  mvn test -P bstack-parallel -Dbrowserstack.config="./src/test/resources/conf/browserstack-parallel.yml"
  ```


Gradle:
  ```sh
  gradle clean bstack-parallel -Dbrowserstack.config="./src/test/resources/conf/browserstack-parallel.yml"
  ```


- Output

  This run profile executes the entire test suite in parallel on a single BrowserStack browser. Please refer to your [BrowserStack dashboard](https://automate.browserstack.com/) for test results.


### Run the entire test suite in parallel on multiple BrowserStack browsers

In this section, we will run the tests in parallel on multiple browsers on Browserstack. Refer to the [`browserstack-parallel-browsers.yml`](src/test/resources/conf/browserstack-parallel-browsers.yml) file to change test capabilities for this configuration.

- How to run the test?

  To run the entire test suite in parallel on multiple BrowserStack browsers, use the following command:

  Maven:
  ```sh
  mvn test -P bstack-parallel-browsers -Dbrowserstack.config="./src/test/resources/conf/browserstack-parallel-browsers.yml"
  ```

  Gradle:
  - For \*nix based and Mac machines:
  ```sh
  gradle clean bstack-parallel-browsers -Dbrowserstack.config="./src/test/resources/conf/browserstack-parallel-browsers.yml"
  ```

### [Web application hosted on internal environment] Running your tests on BrowserStack using BrowserStackLocal

#### Prerequisites

- Clone the [BrowserStack demo application](https://github.com/browserstack/browserstack-demo-app) repository.
  ```sh
  git clone https://github.com/browserstack/browserstack-demo-app
  ``` 
- Please follow the README.md on the BrowserStack demo application repository to install and start the dev server on localhost.
- In this section, we will run a single test case to test the BrowserStack Demo app hosted on your local machine i.e. localhost. Refer to the [`browserstack-local.yml`](src/test/resources/conf/browserstack-local.yml) file to change test capabilities for this configuration.
- Note: You may need to provide additional BrowserStackLocal arguments to successfully connect your localhost environment with BrowserStack infrastructure. (e.g if you are behind firewalls, proxy or VPN).
- Further details for successfully creating a BrowserStackLocal connection can be found here:

  - [Local Testing with Automate](https://www.browserstack.com/local-testing/automate)
  - [BrowserStackLocal Java GitHub](https://github.com/browserstack/browserstack-local-java)


### [Web application hosted on internal environment] Run a specific test on BrowserStack using BrowserStackLocal

- How to run the test?

  - To run the default test scenario (e.g. End to End Scenario) on a single BrowserStack browser using BrowserStackLocal, use the following command:

  Maven:

  ```sh
  mvn test -P bstack-local -Dbrowserstack.config="./src/test/resources/conf/browserstack-local.yml"
  ```

  Gradle:
  ```sh
  gradle clean bstack-local -Dbrowserstack.config="./src/test/resources/conf/browserstack-local.yml"
  ```

  To run a specific test scenario, use the following command with the additional test-name argument:

  Maven:
  ```sh
  mvn test -P bstack-local -Dbrowserstack.config="./src/test/resources/conf/browserstack-local.yml" -Dtest-name="<Test scenario name>"
  ```

  Gradle:
  ```sh
  gradle clean bstack-local -Dbrowserstack.config="./src/test/resources/conf/browserstack-local.yml" -Dtest-name="<Test scenario name>"
  ```

  where,  the argument 'test-name' can be any scenario name configured in this repository.

  E.g. "Login as username", "Login as Locked User", "Offers for mumbai geo-location" or any of the other test scenario names, as outlined in [About the tests in this repository](#About-the-tests-in-this-repository) section.


- Output

  This run profile executes a single test on an internally hosted web application on a single browser on BrowserStack. Please refer to your BrowserStack dashboard(https://automate.browserstack.com/) for test results.


### [Web application hosted on internal environment] Run the entire test suite in parallel on a single BrowserStack browser using BrowserStackLocal

In this section, we will run the test cases to test the internally hosted website in parallel on a single browser on Browserstack. Refer to the [`browserstack-local-parallel.yml`](src/test/resources/conf/browserstack-local-parallel.yml) file to change test capabilities for this configuration.

- How to run the test?

  To run the entire test suite in parallel on a single BrowserStack browser using BrowserStackLocal, use the following command:

  Maven:

  ```sh
  mvn test -P bstack-local-parallel -Dbrowserstack.config="./src/test/resources/conf/browserstack-local-parallel.yml"
  ```

  Gradle:
  ```sh
  gradle clean bstack-local-parallel -Dbrowserstack.config="./src/test/resources/conf/browserstack-local-parallel.yml"
  ```

- Output

  This run profile executes the entire test suite on an internally hosted web application on a single browser on BrowserStack. Please refer to your [BrowserStack dashboard](https://automate.browserstack.com/) for test results.


### [Web application hosted on internal environment] Run the entire test suite in parallel on multiple BrowserStack browser using BrowserStackLocal

In this section, we will run the test cases to test the internally hosted website in parallel on multiple browsers on Browserstack. Refer to the [`browserstack-local-parallel-browsers.yml`](src/test/resources/conf/browserstack-local-parallel-browsers.yml) file to change test capabilities for this configuration.

- How to run the test?

  To run the entire test suite in parallel on a single BrowserStack browser using BrowserStackLocal, use the following command:

  Maven:

  ```sh
  mvn test -P bstack-local-parallel-browsers -Dbrowserstack.config="./src/test/resources/conf/browserstack-local-parallel-browsers.yml"
  ```

  Gradle:
  ```sh
  gradle clean bstack-local-parallel-browsers -Dbrowserstack.config="./src/test/resources/conf/browserstack-local-parallel-browsers.yml"
  ```

- Output

  This run profile executes the entire test suite on an internally hosted web application on multiple browsers on BrowserStack. Please refer to your [BrowserStack dashboard](https://automate.browserstack.com/) for test results.


## Notes
* You can view your test results on the [BrowserStack Automate dashboard](https://www.browserstack.com/automate)
