{% extends "de_docs-content.tmpl" %}
{% block primary %}

Integration in ein Build-System
===============================

Wird ReTest bspw. in ein Build-System integriert, so sollte die Ausführung nicht über die GUI erfolgen.
Generell ist es sinnvoll, ReTest dauerhaft in Ihr Build-System zu integrieren. 
Dazu gibt es derzeit drei Möglichkeiten:

1. Ausführung direkt über die [Kommandozeile](https://de.wikipedia.org/wiki/Kommandozeile).
2. Ausführung über [Ant](http://ant.apache.org).
3. Ausführung über [Maven](https://maven.apache.org).

Durch die Möglichkeit der Ausführung über die Kommandozeile, ist die Integration in gängige Build-Systeme problemlos möglich.
Falls Sie hierbei Unterstützung benötigen, so sprechen Sie uns gerne an.
Haben Sie eine Integration durchgeführt, so lassen Sie es uns wissen, damit wir dies hier dokumentieren können.

Bei allen drei Schnittstellen stehen Ihnen folgende Befehle zur Verfügung:

1. Konvertieren von Suites
2. Generieren von Suites
3. Abspielen von Suites
4. Automatisches Updaten von ReTest 
5. Migrieren von Dateien nach einem ReTest-Update

## Ausführung über die Kommandozeile

Die Ausführung über die Kommandozeile erfolgt über den `java`-Befehl. Folgendes Shell-Skript startet beispielsweise das Abspielen aller ExecSuites:

```
# ReTest installation directory.
retest_home="./"
# ReTest workspace containing actions, tests, suites, and license.
retest_workspace="$retest_home/../retest-workspace"
# Properties file to configure ReTest.
properties="$retest_workspace/retest.properties"
# System under test (SUT), i.e. the application you would like to test.
system_under_test="$retest_home/../system-under-test"

cd "$system_under_test"

javaArgs=" -Dde.retest.configFile=$properties -Dsun.java2d.d3d=false -Dsun.java2d.xrender=false -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=$retest_workspace -XX:-OmitStackTraceInFastThrow -Xms1g "
javaAgent=" -javaagent:$retest_home/retest.jar "
paths=" -Dde.retest.workDirectory=$retest_workspace -Dde.retest.Dir=$retest_home -Dlogback.configurationFile=$retest_workspace/logback.xml "

mkdir -p "$retest_workspace/logs"

exec java $javaArgs $javaAgent $paths -cp "$retest_home/retest.jar" de.retest.TestReplayer
```

Bzw. auf Windows:

```
:: ReTest installation directory.
SET "retest_home=%~dp0"
:: ReTest workspace containing actions, tests, suites, and license.
SET "retest_workspace=%retest_home%\..\retest-workspace"
:: Properties file to configure ReTest.
SET "properties=%retest_workspace%\retest.properties"
:: System under test (SUT), i.e. the application you would like to test.
SET "system_under_test=%retest_home%\..\system-under-test"

cd %system_under_test%

SET "javaArgs= -Dde.retest.configFile=%properties% -Dsun.java2d.d3d=false -Dsun.java2d.xrender=false -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=%retest_workspace% -XX:-OmitStackTraceInFastThrow -Xms1g "
SET "javaAgent= -javaagent:%retest_home%\retest.jar "
SET "paths= -Dde.retest.workDirectory=%retest_workspace% -Dde.retest.Dir=%retest_home% -Dlogback.configurationFile=%retest_workspace%\logback.xml "

IF NOT EXIST "%retest_workspace%\logs" MKDIR %retest_workspace%\logs

java %javaArgs% %javaAgent% %paths% -cp %retest_home%\retest.jar de.retest.TestReplayer
```

Bei der Ausführung über die Kommandozeile können Sie grundsätzlich folgende Klassen verwenden:

1. `de.retest.TestConvert` zum Konvertieren von Suites.
2. `de.retest.TestGenerator` zum Generieren von Tests und zum Monkey Testing.
3. `de.retest.TestReplayer` zum Abspielen von Suites.
4. `de.retest.UpdateReTest` zum Aktualisieren von ReTest.
5. `de.retest.TestMigrator` zum Migrieren von ReTest-Dateien nach einem Update.

In den Fällen 1, 2 und 3 wird als Argument eine Liste Suites (1) bzw. ExecSuites (2 und 3), getrennt durch Semikolon, erwartet. Wird kein Argument übergeben, so werden alle Suites bzw. ExecSuites verwendet. Zur Migration können beliebige Dateien (Actions, Tests und ExecSuites), ebenfalls getrennt durch Semikolon, übergeben werden. Wird hierbei kein Argument mitgeliefert, dann werden alle Actions, Tests und ExecSuites migriert. Im Falle eines Updates (5) ist kein Argument notwendig.

## Ausführung über Ant

ReTest kann ebenfalls via Ant ausgeführt werden. 
Hierfür stehen für die o. g. Aufgaben (Aufnehmen, Abspielen, Generieren etc.) vorgefertigte Tasks zur Verfügung. 
Ein entsprechendes funktionierendes Beispiel findet sich in der ReTest-Demo (https://update.retest.de/demo) in der `build.xml` (im ReTest-Workspace), welche Sie für Ihre eigenen Zwecke adaptieren können.

Die Tasks zum Konvertieren, Abspielen und Generieren können mit [Ant-FileSets](https://ant.apache.org/manual/Types/fileset.html) sehr flexibel konfiguriert werden.

## Ausführung über Maven

*TODO*

{% endblock primary %}
