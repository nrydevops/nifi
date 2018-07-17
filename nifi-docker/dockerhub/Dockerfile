# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements. See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership. The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License. You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied. See the License for the
# specific language governing permissions and limitations
# under the License.
#

FROM nrydevops/nifi
LABEL maintainer "nrydevops rybruscoe@gmail.com"

USER root

#Install Java JDK
RUN mkdir -p /opt/java
RUN apt-get update -y
RUN apt-get install default-jdk -y

#Install Maven
RUN apt-get install maven -y

#Install Geomesa NiFi
RUN apt install git -y
RUN git clone --branch geomesa-nifi-2.0.2 https://github.com/geomesa/geomesa-nifi.git

#Build and Install the Geomesa NiFi Processors
WORKDIR geomesa-nifi
RUN mvn clean install
RUN wget -O geomesa-tools_2.11-2.0.2-data.jar http://search.maven.org/remotecontent?filepath=org/locationtech/geomesa/geomesa-tools_2.11/2.0.2/geomesa-tools_2.11-2.0.2-data.jar
RUN cp /opt/nifi/nifi-1.6.0/geomesa-nifi/geomesa-nifi-nar/target/geomesa-nifi-nar-2.0.2.nar /opt/nifi/nifi-1.6.0/lib/

# Web HTTP Port & Remote Site-to-Site Ports
EXPOSE 8080 8181

WORKDIR $NIFI_HOME

# Startup NiFi
ENTRYPOINT ["bin/nifi.sh"]
CMD ["run"]
