# WeatherCheck
Ubiquitous Computing - Dokumentation - Tobias Wulff - (Update: 11.08.2023)


# Einführung

In unserer heutigen Gesellschaft ist Zeitersparnis oft ein wichtiges Thema. Bevor wir vor die Tür gehen, checken wir mit einer unserer Apps auf dem Handy, wie das Wetter wird. <br>Aber wie wäre es mit einer Alternative für zu Hause?
<br>Eine, die uns wieder etwas weg vom Smartphone führt?


# Problemstellung

Heutzutage gibt es für fast alles eine App, aber ist das wirklich gut? Viele Menschen verbringen bereits einen Großteil ihrer Zeit mit ihrem Smartphone. Vor allem bei der jüngeren Generation kann man manchmal beobachten, wie manche ihr normales Leben fast vergessen, weil sie zu tief darin versunken sind. Aber gibt es eine Möglichkeit, diesem Problem entgegenzuwirken? Es wäre schön, wenn Problemlösungen wieder mehr wären als eine App auf dem Handy.<br>
Um das Problem auf einen kleineren Bereich einzugrenzen, soll hier der Fokus auf Wetter-Apps gelegt werden. Eine bisherige Alternative dazu sind Wetterberichte in Zeitungen, Radio oder Fernsehen sowie physische Displays zur Anzeige lokaler Wetterdaten. Interessant ist die Frage, wie man diese physischen Wetterdisplays etwas spannender machen kann oder vielleicht auch dafür sorgen kann, dass sie nicht einfach nur einen weiteren Platz im Regal wegnehmen.


# Lösungsansatz

Eine prominente Idee zur Lösung des Problems war, die Anzeige von Wetterdaten intuitiv in andere Alltagsgegenstände zu integrieren, ohne diese mit Informationen zu überfrachten.

## Kombination: Uhr

Diese Kombination hat den Vorteil, dass für einen Großteil der Nutzer Wetterdaten wie Temperatur, Bewölkungsgrad, Regenwahrscheinlichkeit meist im Zusammenhang mit der aktuellen Uhrzeit benötigt werden.
Außerdem ist ein Blick auf die Uhr an der Wand durchaus intuitiver, häufiger und etwas einfacher, als das Handy zu entsperren und die entsprechende App oder das Widget zu suchen.


# Idee und Skizzen

Verschiedene Ideen zur farblichen Darstellung von Wetter- und Tageszeitinformationen:
![image](https://i.imgur.com/MMtGajV.jpg)


# Planung

## Software

Die Software sollte zunächst in der Lage sein, eine WLAN-Verbindung aufzubauen. Diese sollte für den Benutzer möglichst einfach einstellbar sein.<br> Sollte es nicht möglich sein, den Standort über die IP herauszufinden und zu bestimmen, sollte der Nutzer diesen ebenfalls einfach angeben und auch ändern können. Mit Hilfe des Standortes können dann die lokalen Wetterdaten der nächsten 12 Stunden von der API abgerufen werden. Diese Daten werden dann klassifiziert und gespeichert. Standardmäßig werden die Temperaturdaten zunächst in Signale für die LEDs des Pixelrings übersetzt und angezeigt. Mit Hilfe des internen Mikrofons des Arduinos soll durch die Erkennung von Schlüsselwörtern wie z.B. "Temperatur", "Niederschlag", "Sonnenaufgang" oder "Sonnenuntergang" der Modus der LED-Anzeige geändert werden. Je nach aktivem Modus werden dann unterschiedliche Informationen durch unterschiedliche Farbschemata dargestellt, um eine Informationsüberflutung zu vermeiden.

## Hardware

Zunächst verwendet wird ein NeoPixel Ring, der den Rahmen einer handelsüblichen Wanduhr bilden soll. Dieser würde Informationen der nächsten 12 Stunden wie Temperatur, Niederschlag, Bewölkung oder Sonnenauf- und -untergang farbcodiert anzeigen.<br>
Um alle nötigen Informationen zu beschaffen und zu verarbeiten wird ein Arduino vom Typ Nano RP2040 Connect verwendet. Dieser soll entweder mit einer Batterie oder einem Netzteil betrieben werden.<br>
Der Pixel Ring benötigt ebenfalls eine eigene 5V Versorgung.


# Umsetzung

Zunächst war es wichtig, überhaupt an Wetterdaten zu gelangen und herauszufinden, in welchem Format diese zur Verfügung stehen. Diese Aufgabe übernahm zunächst ein [eigenständiges Python-Skript](python/AutoFetchWeather.pyw), das stündlich im Hintergrund die lokalen Wetterdaten kostenlos von der API der Webseite open-meteo.com bezog.
<br>
Um diesen Dienst auf dem Arduino auszuführen, musste zunächst eine Verbindung zum Internet hergestellt werden. Dazu wurde eine Modifikation eines [Beispielskripts für WifiNINA](ino/WeatherCheck.ino) verwendet.
<br>
Im nächsten Schritt wurde versucht, die beiden Skripte zu kombinieren, indem das Python-Skript in C++ umgewandelt wurde. Es gab jedoch einige Probleme, auf diese Weise eine vernünftige sichere Verbindung zur Website herzustellen. Außerdem erwies es sich sich als wesentlich aufwendiger, auf diese Weise die Wetterdaten geordnet zwischenzuspeichern und sie zu verarbeiten.
<br>
Um dieses Problem zu lösen, spielte die Verwendung von MicroPython in der Entwicklungsumgebung "Thonny" eine Rolle. MikroPython läuft auf dem Arduino genauso wie C++, nur eben in Python. Ein kurzes [Skript zum Testen](thonny/blink_test.py) der Lauffähigkeit befindet sich im Verzeichnis.


# Zukunft

Der nächste Schritt wäre nun, eine funktionierende Internetverbindung über MicroPython herzustellen und die Wetterdaten herunterzuladen. Anschließend müsste eine statische Tabelle erstellt werden, um die gewonnenen Daten den jeweiligen Farbwerten für die LEDs zuzuordnen. Um diese auf dem NeoPixel Ring anzeigen zu können, müsste zunächst noch eine 5V-Versorgung für den NeoPixel Ring sichergestellt werden.<br>
Mit Hilfe von MicroPython soll das Arduino-Mikrofon dazu verwendet werden, die Schlüsselbegriffe zu erkennen und dann den jeweiligen Modus der LED-Ringe zu ändern, um die entsprechenden Informationen anzuzeigen.<br>
Zum Schluss muss noch die Wanduhr mit dem Arduino und dem NeoPixel Ring kombiniert werden.

# Quellen

- [Getting Started with MicroPython](https://projects.raspberrypi.org/en/projects/getting-started-with-the-pico/0)
- [WiFiNINA](https://www.arduino.cc/reference/en/libraries/wifinina/)
- [SpeechRecognition](https://pypi.org/project/SpeechRecognition/)
- [Ardunio Nano RP2040 Connect Docs](https://docs.arduino.cc/hardware/nano-rp2040-connect)

