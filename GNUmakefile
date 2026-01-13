SHELL := bash
.DEFAULT_GOAL := help

TOMCAT_WEBAPPS := /opt/tomcat/webapps
HARVESTER_WAR_DIR := $(TOMCAT_WEBAPPS)/harvester
AUTH_DIR := /opt/tomcat/conf/authentication

help:
	@echo "Harvester Server 3.0.2"
	@echo ""
	@echo "make build            Build Geoportal WAR"
	@echo "make deploy           Deploy exploded WAR (hot reload)"
	@echo "make clean            Clean Tomcat deployment"
	@echo "make auth-simple      Switch to simple authentication"
	@echo "make auth-keycloak    Switch to Keycloak authentication"
	@echo "make logs             Tail Tomcat logs"

build:
	mvn -f ./pom.xml clean package -DskipTests

deploy: build
	rm -rf $(HARVESTER_WAR_DIR)
	mkdir -p $(HARVESTER_WAR_DIR)
	unzip -q geoportal-application/geoportal-harvester-war/target/*.war -d $(HARVESTER_WAR_DIR)
	@echo "✔ Harvester deployed (exploded WAR)"

clean:
	rm -rf $(HARVESTER_WAR_DIR)
	rm -rf /opt/tomcat/logs/*
	@echo "✔ Tomcat harvester cleaned"

auth-simple:
	@echo "authentication-simple.xml" > /tmp/gpt_auth
	export GPT_AUTHENTICATION=authentication-simple.xml
	@echo "✔ Switched to SIMPLE auth (restart Tomcat)"

auth-keycloak:
	@echo "authentication-keycloak.xml" > /tmp/gpt_auth
	export GPT_AUTHENTICATION=authentication-keycloak.xml
	@echo "✔ Switched to KEYCLOAK auth (restart Tomcat)"

logs:
	tail -f /opt/tomcat/logs/catalina.out
