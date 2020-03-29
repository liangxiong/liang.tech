# maven

mvn clean package install -Dmaven.test.skip=ture -DskipTests
mvn clean package -Dmaven.test.skip=ture -DskipTests
mvn dependency:tree
mvn dependency:list
mvn dependency:analyze

mvn help:effective-pom

-XX:PermSize=128m -XX:MaxPermSize=256m
