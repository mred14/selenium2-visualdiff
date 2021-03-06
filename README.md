Selenium Visual Diff
====================

Goal
----
The goal is a better integration of screenshots taking in maven executed *selenium2*
functional tests, storing and versioning of screenshots to get a report of visual differences
between two application versions.

Report
------
See sample report http://kreyssel.github.com/selenium2-visualdiff/visualdiff.html.

Usage
-----
Embed *jUnit4* and the *selenium2-visualdiff* core library as dependencies in your funtional test maven module:

    <dependencies>
        ...
        <dependency>
            <groupId>org.kreyssel.selenium2.visualdiff</groupId>
            <artifactId>visualdiff-core</artifactId>
            <version>1.0.0-SNAPSHOT</version>
            <scope>test</scope>
        </dependency>
        ...
    </dependencies>

Add the selenium2-visualdiff-maven-plugin to the maven module:

    <plugins>
        ...
        <plugin>
            <groupId>org.kreyssel.selenium2.visualdiff</groupId>
            <artifactId>visualdiff-maven-plugin</artifactId>
            <version>1.0.0-SNAPSHOT</version>
            <executions>
                <execution>
                    <goals>
                        <goal>prepare</goal>
                        <goal>package</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
        ...
    </plugins>

And after all, embed *org.kreyssel.selenium2.visualdiff.core.junit4.TakesScreenshotRule* in your functional test:

    package org.kreyssel.selenium2.visualdiff.it;
    
    import org.kreyssel.selenium2.visualdiff.core.junit4.TakesScreenshotRule;
    
    /**
     * SimpleSeleniumIT.
     */
    public class SimpleSeleniumIT {
    
        @Rule
        public TakesScreenshotRule screenshot = new TakesScreenshotRule();
      
        RemoteWebDriver driver;
      
        @Before
        public void init() {
          driver = createDriver();
        }
      
        @After
        public void destroy() {
          driver.close();
        }
          
        @Test
        public void test1() throws Exception {
            driver.get( "http://localhost:8080" );
    
            screenshot.takeScreenshot( driver );
        }
    }

After the executions of functional tests in a maven run, all screenshots taken at this time are packaged as zip and attached to the build as *${project.build.finalName}-screenshots.zip*. This archive is deployed to maven repository in the *deploy* phase of the maven build.

The *selenium2-visualdiff* report plugin generates a report that shows you the different screens per testcase compared to the previous release.

