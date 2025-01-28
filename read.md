# Documentation: Custom Object Mapper and Script Connector in Camunda

## Goal
The objective is to:
1. Verify the functionality of **Custom Object Mapper** in the [Camunda DMN Scala library](https://github.com/camunda/dmn-scala/).
2. Implement a sample example using the [Script Connector](https://github.com/camunda-community-hub/script-connector) and understand its integration with local JS files.

## Task 1
1. In the [Camunda DMN Scala](https://github.com/camunda/dmn-scala/) repository, the dependency is available. 
```xml
<dependency>
  <groupId>org.camunda.bpm.extension.dmn.scala</groupId>
  <artifactId>dmn-engine</artifactId>
  <version>${version}</version>
</dependency>
```
2. I sent and fetched the data from DMN table created in a bpmn file. By default, its catching the data from DMN in java data type.
3. The code was tested and its auto-converting the FEEL data type to JAVA data type (e.g. number to double (4.5 (number) -> 4.5 (double))  .
4. Have added the toVal and unpackVal functionalities, but as its auto-identifying the data type in springbot, the use-case was not as expected.

## Task 2
1. Cloned the [Script Connector repository](https://github.com/camunda-community-hub/script-connector).
2. Published the script connector and ran the jar file in local.
3. There are 2 options to integrate the separate language's script (i.e. Resources and Embedded). 
   - In Embedded, the engine is not reading the Script and language input fields.
   - In Resources, the engine is reading the JS file(which is present in local), but not able to execute. 
4. Only way we found to execute the code is to update the value in XML file itself and deploy it. Here the engine is reading and executing the JS code.
```xml
<zeebe:taskHeaders>
	<zeebe:header key="resultVariable" value="result" />
	<zeebe:header key="language" value="javascript" />
	<zeebe:header key="script" value="c=5; console.log(a+b+c)" />
	<zeebe:header key="retryBackoff" value="PT0S" />
</zeebe:taskHeaders>
```
5. Verdict : The connector is not able to read scripts with path or script provided

## Conclusion
The script connector has some issues and has limited use cases (Inline code is working fine, reading from local files is not working as expected).
The object valueMapper is already being executed if we add the dependency in springboot.
