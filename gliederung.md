Effektive Go als Kurs

Die Bibel aller Go (Golang) Entwickler

[[TOC]]

# Einleitung

## Über diesen Kurs

## Kursunterlagen

* Diese Gliederung: [https://goo.gl/mkXWiX](https://goo.gl/mkXWiX) 
* [Effective Go](https://golang.org/doc/effective_go.html)
* [Effektiv Go programmieren](http://bitloeffel.de/DOC/golang/effective_go_de.html) (deutsche Übersetzung)
* [https://github.com/as27/effective_go](https://github.com/as27/effective_go)
    * PullRequests willkommen
    * Ziel: Bessere Community Übersetzung

## Tipps für den Kurs

* Die Videos können schneller abgespielt werden. 
    * Aufnahmefähigkeit ist schneller als meine Erzählgeschwindigkeit.
    * Diese Methode ist nicht für jeden etwas, deshalb einfach mal ausprobieren
* Beim Ansehen des Video
    * "b" drücken, um ein Lesezeichen zu setzen
* Feedback geben
* Bei Problemen einfach eine Frage hier im Kurs stellen

## Idiomatischer Go Code

* Effective Go beschreibt idiomatischen Go Code
* Eine Art wie die Programmiersprache verwendet werden soll
* Entspricht einer Empfehlung
* Ziel bessere Zusammenarbeit mehrerer Entwickler

## Umgang mit der Dokumentation

* [https://golang.org/pkg/](https://golang.org/pkg/) 
* [https://godoc.org/](https://godoc.org/) 
* Gliederung
    * Überblick
    * Index
        * Gegliedert nach Typen
    * Beispiele

# Formatierung

* Einigung über Formatierungsregeln kosten Zeit und Energie
* In Go verwenden wir gofmt
* Fehlende Details
    * **Einrückungen**: Default werden Tabs gesetzt. Leerzeichen erlaubt, sollten jedoch nur wenn unbedingt notwendig angewendet werden.
    * **Zeilenlänge**: Keine Einschränkung. Zeilen können umgebrochen werden und danach wird ein extra Tab gesetzt.
* Beispiel im Go Playground
* Beispiel VSCode

# Kommentare

* Block-Kommentare `/* */` C-Stil
    * Für Paket-Beschreibungen
    * Deaktivieren von Code
* Zeilenkommentare `//` C++ Stil
    * Standardfall
* Programm godoc (auch Webserver)
* Paketkommentar
    * vor der Paket Deklaration
    * bei mehreren Dateien kann dies in einer beliebigen Datei erfolgen
    * Soll das Paket vorstellen
    * Wird von godoc als erstes generiert
    * I.d.R. als Blockkommentar bei einfachen Paketen auch einfacher möglich
* Werden in godoc als Text interpretiert
    * Keine Formatierung oder HTML in die Kommentare schreiben
* Doc Kommentar
    * Vor der Deklaration
    * Als ganzer Satz erstes Wort soll der Name des jeweiligen Elements sein
* Kommentare bei Variablen Gruppen
    * Einfache Formulierung, da für die ganze Gruppe gültig
* Exkurs: Gruppierung von Variablen kann auch Beziehungen zwischen Variablen darstellen

# Namen

* Exportierte Elemente beginnen immer mit einem Großbuchstaben. 
* In anderen Sprachen gibt es das Keyword public
* In Go: "exported" und “unexported”
* Analog zu objektorientierten Sprachen "public" und “private”

## Paketnamen

* Einfache kurze Namen
* Kleinschreibung
* Keine Angst vor Namenskonflikten, nur weil ein anderes Paket bereits den geeignetsten Namen verwendet
    * Namespace für das Paket kann überschrieben werden
* der Paketname wird über das Quellverzeichnis definiert
* Exportierte Elemente eines Pakets sollen immer in Verbindung mit dem Paketnamen betrachtet werden
    * die Verwendung ist: bufio.Reader
    * Deshalb ist z.B. BufReader nicht gewünscht
* Kurze Funktionsnamen

## Getter

* Go hat keine automatische Unterstützung für Getter und Setter
* Get nicht im Funktionsnamen 
    * Idiomatisch: `obj.Owner()`
    * Nicht idiomatisch: `obj.GetOwner()`
* Setter dürfen Set im Namen haben
    * `obj.SetOwner()`

## Interface-Namen

* Ein-Methoden-Interface
    * -er Suffix wird an den Funktionsnamen gehängt
    * `Read()` -> Reader
    * `Write()` -> Writer
* Funktionen mit bereits anerkannten Signaturen
    * `Read`, `Write`, `Close`, `Flush`, `String`
    * Bei verwendung dieser Namen sollen die üblichen Signaturen eingehalten werden
    * Bei der Implementierung soll auch der übliche Name verwendet werden
        * String Converter (Stringer Interface)
            * idiomatisch: `String()`
            * nicht idiomatisch: `ToString()`

## MixedCaps

Variablen oder Funktionen aus längeren Wörtern werden mit MixedCaps geschrieben

* idiomatisch: `ReadWriter`
* nicht idiomatisch: `Read_Writer`

# Semikolons

* die formale Go Grammatik verwendet Semikolons 
* sind im Code nicht sichtbar
* werden durch den Lexer automatisch hinzugefügt
* Im Code nur bei for loops
* öffnende geschweifte Klammern dürfen nicht in einer neuen Zeile stehen

# Kontrollstrukturen

* Verwand mit C
    * kein do oder while
    * erweiterte if und switch Anweisungen
        * optionale Initialisierung (Scope liegt dann innerhalb der Anweisung)
    * break und continue akzeptieren optional ein Label

## If

* benötigt geschweifte Klammern
* guter Stil, wenn if über mehrere Zeilen läuft
* Initialisierung von Variablen. Achtung Scope ist nur innerhalb des if Statements!
* idiomatic: else wird weggelassen, wenn break, continue, goto oder return folgt
    * gilt insbesondere auch für Fehlerbehandlung

## Redeklaration und mehrmalige Zuweisung

* Go Funktionen können mehrere Rückgabewerte besitzen
* Kurzdeklaration definiert beide Variablen
* f, err := os.Open(name)

```go
// nicht erlaubt
err := myFunc()
// erlaubt
d, err := f.Stat()
```

* mehrmalige Deklarationen sind erlaubt wenn:
    * die Variable im gleichen Scope ist (ansonsten würde eine neuer Speicherbereich/Variable dafür angelegt werden)
    * der Wert der neuen Deklaration kann dem vorhandenen Typ zugewiesen werden
    * mindestens eine neue Variable wird durch die Kurzdeklaration neu definiert
* Grund: für die Fehlerbehandlung soll immer einheitlich err verwendet werden.

## For

In Go gibt es nur for für die Definition von Schleifen. while oder do-while sind hier nicht vorhanden.

* Arten von for
    * Wie for in C  
    `for init; condition; post {}`
    * Wie while in C  
    `for condition {}`
    * Wie for(;;;) in C  
    `for {}`
* Definition der Index Variablen über Kurzdeklaration  
`for i := 0; i < 10; i++ { … }`
* Loops über array, slice, string, map oder das Auslesen eines channels werden mit range abgebildet  
`for key, value := range oldMap { … }`
    * wenn nur der erste Wert gebraucht wird  
    `for key := range oldMap { … }`
    * wenn nur der zweite Wert gebraucht wird  
    `for _, value := range oldMap { … }`
* Range über Text
    * Ein Buchstabe kann in UTF-8 mehr als ein byte benötigen
        * [https://www.youtube.com/watch?v=MijmeoH9LT4](https://www.youtube.com/watch?v=MijmeoH9LT4) (Computerphile)
    * In Go wir ein Buchstabe als rune abgebildet

```go
for pos, char := range "日本\x80語" { // \x80 is an illegal UTF-8 encoding
    fmt.Printf("character %#U starts at byte position %d\n", char, pos)
}
```
Ergibt folgenden Output
```
character U+65E5 '日' starts at byte position 0
character U+672C '本' starts at byte position 3
character U+FFFD '�' starts at byte position 6
character U+8A9E '語' starts at byte position 7
```

* Abbildung mehrerer Index Variablen
    * multiple Zuweisung von Go kann verwendet werden  
`for i, j := 0, len(a)-1; i < j; i, j = i+1, j-1 {`

## Switch

* Allgemeiner als in C
* break muss nicht verwendet werden. Wenn ein case zutrifft, dann wird switch auch beendet.
    * möchte man trotzdem weitere Fälle prüfen verwendet man fallthrough
* switch ohne Bedingung
    * der Ausdruck unter case wird interpretiert

```go
func unhex(c byte) byte {
    switch {
    case '0' <= c && c <= '9':
        return c - '0'
    case 'a' <= c && c <= 'f':
        return c - 'a' + 10
    case 'A' <= c && c <= 'F':
        return c - 'A' + 10
    }
    return 0
}
```

* switch mit Bedingung

```go
switch user.Role {
case "admin":
	…
}
```

Die unterschiedlichen Fälle können auch kommasepariert in einem case angegeben werden

```go
func shouldEscape(c byte) bool {
    switch c {
    case ' ', '?', '&', '=', '#', '+', '%':
        return true
    }
    return false
}
```

Break kann auch innerhalb von switch verwendet werden, wenn bestimmte Fälle frühzeitig abgebrochen werden müssen.

Zur break anweisung kann zusätzlich ein Label angegeben werden, das break bezieht sich dann auf den Loop des Labels.

Folgendes Beispiel beinhaltet verschiedene Fälle

```go
Loop:
	for n := 0; n < len(src); n += size {
		switch {
		case src[n] < sizeOne:
			if validateOnly {
				break
			}
			size = 1
			update(src[n])

		case src[n] < sizeTwo:
			if n+1 >= len(src) {
				err = errShortInput
				break Loop
			}
			if validateOnly {
				break
			}
			size = 2
			update(src[n] + src[n+1]<<shift)
		}
    }
```

## Type-Switch

* Der Type Switch kann verwendet werden, um den Typ eines Interfaces zu bestimmen.
* Gilt nicht nur für leere Interfaces:
[https://play.golang.org/p/HF6XGpDDHCS](https://play.golang.org/p/HF6XGpDDHCS) 
* switch v.(type) { … }

# Funktionen

## Mehrere Rückgabewerte

* eine Funktion kann mehrere Werte zurückgeben
    * Ziel klare Trennung zwischen Wert und Fehler bzw. Erfolgsmeldung
    * func (file *File) Write(b []byte) (n int, err error)
* Weitere Anwendung, wenn Zahl und ein Pointer zurückgeliefert wird
    * [https://play.golang.org/p/TNpG4x_NloR](https://play.golang.org/p/TNpG4x_NloR) 

## Benannter Rückgabewert

* Rückgabewerte/Ergebnisvariablen können analog wie die Eingangswerte auch mit einem Namen versehen werden
* In dem Fall wird am Anfang eine Variable mit Null Wert initialisiert
* Danach wird nur das Keyword return verwendet (analog Visual Basic)
* [https://play.golang.org/p/omBLyrGtETC](https://play.golang.org/p/omBLyrGtETC) 
* [https://www.calhoun.io/using-named-return-variables-to-capture-panics-in-go/](https://www.calhoun.io/using-named-return-variables-to-capture-panics-in-go/) 

## Defer

* Defer wird am Ende einer Funktion ausgeführt
* Bei mehreren "LIFO" last in first out
    * [https://play.golang.org/p/zyXqG8Al2-2](https://play.golang.org/p/zyXqG8Al2-2) 
* Anwendung:
    * Freigeben von gebundenen Ressourcen z.B. Close()
    * Aufruf von Funktionen am Ende einer Funktion
        * Beispiel trace und untrace: [https://play.golang.org/p/wHGsA5ogYcp](https://play.golang.org/p/wHGsA5ogYcp) 
    * recover bei einer panic()

# Daten

## Speicherbereitstellung mit new

* zwei Arten für die Bereitstellung von Arbeitsspeicher `new` und `make`
* new
    * liefert einen Pointer auf eine Variable mit dem [Zero-Wert](https://golang.org/ref/spec#The_zero_value)
    * https://play.golang.org/p/g6CV_LKyfMx