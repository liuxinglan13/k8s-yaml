### 2020-07-03

部署jira生产环境记录

jira基础镜像:官方:docker pull atlassian/jira-software:8.1.0

```
FROM library/atlassian-jira-software
COPY atlassian-extras-3.2.jar /opt/atlassian/jira/atlassian-jira/WEB-INF/lib
COPY mysql-connector-java-5.1.49.jar /opt/atlassian/jira/lib
```

* atlassian-extras-3.2.jar 破解文件
* mysql-connector-java-5.1.49.jar mysql驱动程序





