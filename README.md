Skeleton for Swaftee Framework based Projects:
---------------------------------------------
This is a skeleton repositary for Swaftee framework based projects which defines the folder structure for project development. 


Eclipse Setup :
-------------

1. Make sure you have Java version 7 or above installed in your machine.
2. Download eclipse [https://eclipse.org/downloads/].
3. Setup TestNG in eclipse [http://goo.gl/GleaWs].


Project Setup:
-------------

1. Clone the swaftee_skeleton repo from here[https://stash.solutionstarit.com/projects/QA/repos/swaftee_skeleton/browse].
2. Open the eclipse in either new workspace or existing one. 
3. Import the file project by navigating File->import
4. Once the project is imported, verify the java compiler path is set to jdk by following steps below.
   i) Project -> properties
   ii) Click on Java compiler
   iii) Click on Installed JRE's link
   iv) Verify that the path set is for jdk 1.7 or above, otherwise change the path and save.
5. To download the dependencies
   i) Right click on the project module and select Run As -> Maven Install
   ii) Once it completed, right click on the project module and select Maven -> Update Project
6. Rename the repo name with your desired name
7. Update the pom artifact and group id with the changed repo name
8. Do Maven -> Update Project
9. Do mvn clean and mvn install to download the dependencies. 

Executing TestNG as a Java Program:
----------------------------------
The following set-up helps to transform a TestNG xml file into a TestNG Java file. This is particularly useful when the tests need to be run as an executable JAR file in a machine without eclipse installed.
After the

The following TestNG xml suite file,

<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="Localization" verbose="10" preserve-order="true">
	<parameter name="app.domain" value="http://bhhsdemo.rdeskwebsite-stage.com/" />
	<parameter name="maxProperties" value="287571" />
	<parameter name="browser" value="chrome" />
	<test name="Property Details Missing Feature Codes" parallel="instances"
		thread-count="5">
		<classes>
			<class
				name="com.solutionstar.swaftee.redPublic.tests.localization.InputPropsMissedTranslationsUsingChinese" />
		</classes>
	</test>
</suite>


can be transformed into a Java class file as follows:

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import org.testng.TestNG;
import org.testng.xml.XmlClass;
import org.testng.xml.XmlSuite;
import org.testng.xml.XmlTest;

public class TestNGProgrammatically {

	private static void testRunner(Map<String, String> testngParams) {

		// Create an instance on TestNG
		TestNG testNG = new TestNG();

		// Create an instance of XML Suite and assign a name for it.
		XmlSuite suite = getXmlSuite(testngParams);

		// Create an instance of XmlTest and assign a name for it.
		XmlTest test = getXmlTest(suite);

		// Create a list which can contain the classes that you want to run.
		List<XmlClass> classes = getXmlClasses();

		// Assign that to the XmlTest Object created earlier.
		test.setXmlClasses(classes);

		// Create a list of XmlTests and add the Xmltest you created earlier to
		// it.
		List<XmlTest> tests = new ArrayList<XmlTest>();
		tests.add(test);

		// add the list of tests to your Suite.
		suite.setTests(tests);

		// Add the suite to the list of suites.
		List<XmlSuite> suites = new ArrayList<XmlSuite>();
		suites.add(suite);

		// Set the list of Suites to the testNG object you created earlier.
		testNG.setXmlSuites(suites);

		// invoke run() - this will run your class.
		testNG.run();

	}

	private static XmlSuite getXmlSuite(Map<String, String> testngParams) {
		XmlSuite suite = new XmlSuite();
		suite.setName("Localization");
		suite.setVerbose(10);
		suite.setPreserveOrder("true");
		suite.setParameters(testngParams);
		return suite;
	}

	private static XmlTest getXmlTest(XmlSuite suite) {
		XmlTest test = new XmlTest(suite);
		test.setName("Property Details Untranslated Feature Codes");
		test.setParallel("instances");
		test.setThreadCount(5);
		return test;
	}

	private static List<XmlClass> getXmlClasses() {
		List<XmlClass> classez = new ArrayList<XmlClass>();
		classez.add(new XmlClass("com.solutionstar.swaftee.redPublic.tests.localization.InputPropsMissedTranslationsUsingChinese"));
		return classez;
	}

	public static void main(String args[]) {

		Map<String, String> params = new HashMap<String, String>();
		params.put("app.domain", "http://bhhsdemo.rdeskwebsite-stage.com/");
		 params.put("maxProperties", "287571");
		params.put("browser", "chrome");
		testRunner(params);
	}
	
	For the detailed documentation, check: http://testng.org/doc/documentation-main.html#running-testng-programmatically