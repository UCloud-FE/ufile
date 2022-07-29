# Download and environment preparation

## Download and Installation

### Get the adapter

> US3 big data adapter provides its functionality in the form of a jar package. US3 big data adapter jar package format is us3-bigdata-adaptor-${hadoop version}-${adaptor version}.jar.

### adapter version

- v1.3.0
  - [hadoop-2.6.0](http://us3-release.cn-bj.ufileos.com/us3-bigdata/adaptor/v1.3.0/us3-bigdata-adaptor-2.6.0-1.3.0.jar)
  - [hadoop-2.8.3](http://us3-release.cn-bj.ufileos.com/us3-bigdata/adaptor/v1.3.0/us3-bigdata-adaptor-2.8.3-1.3.0.jar)
  - [hadoop-2.8.5](http://us3-release.cn-bj.ufileos.com/us3-bigdata/adaptor/v1.3.0/us3-bigdata-adaptor-2.8.5-1.3.0.jar)
  - [hadoop-3.1.1](http://us3-release.cn-bj.ufileos.com/us3-bigdata/adaptor/v1.3.0/us3-bigdata-adaptor-3.1.1-1.3.0.jar)

**For other versions of Hadoop docking requirements, please contact technical support. **

## Install the adapter

1. configure the core-site.xml parameter entries for each node, refer to [Quick Start - Parameter Description](/ufile/tools/us3hadoop/quickaccess?id=parameter description).
2. copy us3-bigdata-adaptor-${hadoop version}-${adaptor version}.jar to $HADOOP_HOME/share/hadoop/common/lib/.

**Access method is invasive and suitable for small-scale computational analysis scenarios, please refer to [Quick Start-Scenario Example](/ufile/tools/us3hadoop/quickaccess?id=scenario example) for big data backup requirement scenarios. **
