# JMeter 接口测试集成

![alt text](65e12e73-5446-41ae-b658-aff9cd6706eb.png)

## Ant + JMeter
实现多用例并行测试，也可以用Maven或Gradle; ant -f build.xml
* build.xml
```
<?xml version="1.0" encoding="utf-8"?>
<!-- 拷贝报告所需的图片资源至目标目录 -->
<project name="ant-jmeter-test" default="run" basedir=".">
    <tstamp>
        <format property="time" pattern="yyyyMMddHHmm" />
    </tstamp>
    <!-- 需要改成自己本地的jmeter目录 -->
    <property name="jmeter.home" value="/usr/local/apache-jmeter-5.1.1" />
    <!-- jmeter生成的jtl格式的结果报告的路径 -->
    <property name="jmeter.result.jtl.dir" value="./result/jtlfile" />
    <!-- jmeter生成的html格式的结果报告的路径 -->
    <property name="jmeter.result.html.dir" value="./result/htmlfile" />
    <!-- 生成的报告的前缀 -->
    <property name="ReportName" value="TestReport_" />
    <property name="jmeter.result.jtlName" value="${jmeter.result.jtl.dir}/${ReportName}${time}.jtl" />
    <property name="jmeter.result.htmlName" value="${jmeter.result.html.dir}/SummaryReport.html" />
    <property name="jmeter.detail.result.jtlName" value="${jmeter.result.jtl.dir}/${ReportName}${time}.jtl" />
    <property name="jmeter.detail.result.htmlName" value="${jmeter.result.html.dir}/DetailReport.html" />
    <target name="run">
        <antcall target="test" />
        <antcall target="report" />
    </target>
    <target name="test">
        <taskdef name="jmeter" classname="org.programmerplanet.ant.taskdefs.jmeter.JMeterTask" />
        <jmeter jmeterhome="${jmeter.home}" resultlog="${jmeter.result.jtlName}">
            <!-- 声明要运行的脚本*.jmx指包含此目录下的所有jmeter脚本 -->
            <testplans dir="./scripts" includes="*.jmx" />
            <property name="jmeter.save.saveservice.output_format" value="xml" />
        </jmeter>
    </target>
    
    <path id="xslt.classpath">
        <fileset dir="${jmeter.home}/lib" includes="xalan*.jar"/>
        <fileset dir="${jmeter.home}/lib" includes="serializer*.jar"/>
    </path>
    
    <target name="report">
        <tstamp>
            <format property="report.datestamp" pattern="yyyy/MM/dd HH:mm" />
        </tstamp>
        <xslt classpathref="xslt.classpath" force="true"
            in="${jmeter.detail.result.jtlName}"
            out="${jmeter.detail.result.htmlName}"
            style="./jmeter.results.shanhe.me.xsl">
            <param name="dateReport" expression="${report.datestamp}"/>
        </xslt>
        <xslt classpathref="xslt.classpath" force="true"
            in="${jmeter.result.jtlName}"
            out="${jmeter.result.htmlName}"
            style="./jmeter-results-detail-report_21.xsl">
            <param name="dateReport" expression="${report.datestamp}"/>
        </xslt>
        <!-- 拷贝报告所需的图片资源至目标目录 -->
        <copy todir="${jmeter.result.html.dir}">
            <fileset dir="${jmeter.home}/extras">
                <include name="collapse.png" />
                <include name="expand.png" />
            </fileset>
        </copy>
    </target>
</project>
```