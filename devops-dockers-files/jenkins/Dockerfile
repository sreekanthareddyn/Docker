FROM jenkins/jenkins:lts



MAINTAINER Antonio Esposito, <antonio.d.esposito>



ENV GITLAB_HOST_NAME gitlab

ENV GITLAB_PORT 80

ENV GITLAB_SSH_PORT 22

#ENV GITLAB_HOST_NAME, the Gitlab hostname. Default to gitlab
#ENV GITLAB_PORT, the port Gitlab APIs are exposed. Default to 80
#ENV GITLAB_JENKINS_USERNAME, the username Jenkins will use to connect to Gitlab. Default to Gitlab.
#ENV GITLAB_JENKINS_PASSWORD, the password Jenkins will use to connect to Gitlab. Default to gitlab.
#ENV INITIAL_ADMIN_USER, the username for the admin user.
#ENV INITIAL_ADMIN_PASSWORD, the password for the initial admin user.
#ENV LDAP_SERVER, the LDPA URI, i.e. ldap-host:389
#ENV LDAP_ROOTDN, the LDAP BASE_DN
#ENV LDAP_USER_SEARCH_BASE, base organization unit to use to search for users
#ENV LDAP_USER_SEARCH, LDAP object field to use for the search query
#ENV LDAP_GROUP_SEARCH_BASE, base organization unit to use to search for groups
#ENV LDAP_GROUP_SEARCH_FILTER, filter to use querying for groups
#ENV LDAP_GROUP_MEMBERSHIP_FILTER, filter to use when writing queries to verify if a user is member of a group
#ENV LDAP_MANAGER_DN, LDAP adim user
#ENV LDAP_MANAGER_PASSWORD, LDAP admin password
#ENV LDAP_INHIBIT_INFER_ROOTDN, flag indicating if ROOT_DN should be infered
#ENV LDAP_DISPLAY_NAME_ATTRIBUTE_NAME, LDAP object field used as a display name
#ENV LDAP_DISABLE_MAIL_ADDRESS_RESOLVER, flag indicating if the email address resolver should be disabled
#ENV LDAP_MAIL_ADDRESS_ATTRIBUTE_NAME, LDAP object field used as a email address
#ENV LDAP_GROUP_NAME_ADMIN, LDAP admin group. Default to administrators.
#ENV SONAR_SERVER_URL, the sonar server URL
#ENV SONAR_ACCOUNT_LOGIN, username to use when connecting to sonar
#ENV SONAR_ACCOUNT_PASSWORD, password to use when connecting to sonar
#ENV SONAR_DB_URL, sonar database JDBC connection string
#ENV SONAR_DB_LOGIN, username to use to connect to sonar DB
#ENV SONAR_DB_PASSWORD, password to use when connecting to sonar DB
#ENV SONAR_PLUGIN_VERSION, the sonar plugin version
#ENV SONAR_ADDITIONAL_PROPS, additional properties for sonar plugin. Refer to SonarQube documentation for more informattion
#ENV SONAR_RUNNER_VERSION, the sonar runner version
#ENV ANT_VERSION, ANT release version
#ENV MAVEN_VERSION, Maven release version
#ENV NODEJS_VERSION, nodejs release version
#ENV NODEJS_GLOBAL_PACKAGES, nodejs packages to be installed as global
#ENV NODEJS_PACKAGES_REFRESH_HOURS, nodejs package refresh time in hours.
#ENV GIT_GLOBAL_CONFIG_NAME, Git global config name
#ENV GIT_GLOBAL_CONFIG_EMAIL, Git global config email
#ENV ORACLE_DEFAULT_REGION, set the AWS default region for the CLI at a global level
#ENV DOCKER_TLS_VERIFY, Docker CLI variable to declare a TLS-enabled engine
#ENV DOCKER_HOST, Docker CLI variable to declare the endpoint to target
#ENV DOCKER_CERT_PATH, Docker CLI variable to declare the path to the certificate
#ENV DOCKER_NETWORK_NAME, the Docker custom network to launch containers on
#ENV GROOVY_VERSION, a comma delimited list of Groovy installation profiles to install (e.g. 2.4.8, 2.4.3).
#ENVLDAP_IS_MODIFIABLE, allows us to interact with LDAP configuration through jenkins, Allowed values true (default) and false. If set to true, LDAP can be modified and jenkins will be able to create necessary users/groups in LDAP. If set to false, LDAP can not be modified and jenkins need be configured to use existing users/groups in LDAP.

# Copy in configuration files

COPY resources/plugins.txt /usr/share/jenkins/ref/

COPY resources/init.groovy.d/ /usr/share/jenkins/ref/init.groovy.d/

COPY resources/scripts/ /usr/share/jenkins/ref/adop_scripts/

COPY resources/jobs/ /usr/share/jenkins/ref/jobs/

COPY resources/views/ /usr/share/jenkins/ref/init.groovy.d/

COPY resources/m2/ /usr/share/jenkins/ref/.m2

COPY resources/entrypoint.sh /entrypoint.sh

COPY resources/scriptApproval.xml /usr/share/jenkins/ref/



# Reprotect

USER root

RUN chmod +x -R /usr/share/jenkins/ref/adop_scripts/ && \

    chmod +x /entrypoint.sh

# USER jenkins



# Environment variables

ENV LDAP_GROUP_NAME_ADMIN=""

ENV JENKINS_OPTS="--prefix=/jenkins -Djenkins.install.runSetupWizard=false"

ENV PLUGGABLE_SCM_PROVIDER_PROPERTIES_PATH="/var/jenkins_home/userContent/datastore/pluggable/scm"

ENV PLUGGABLE_SCM_PROVIDER_PATH="/var/jenkins_home/userContent/job_dsl_additional_classpath/"



RUN xargs /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

RUN echo "KexAlgorithms curve25519-sha256@libssh.org,ecdh-sha2-nistp384,ecdh-sha2-nistp256,diffie-hellman-group14-sha256,diffie-hellman-group1-sha1" >> /etc/ssh/ssh_config



ENTRYPOINT ["/entrypoint.sh"
