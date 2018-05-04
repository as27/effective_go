# effective_go

Dies ist eine inoffizielle Übersetzung von [Effective Go](https://golang.org/doc/effective_go.html).

## Formatierung

Die Formatierung des Quellcodes wird meistens am stärksten Diskutiert, aber am inkonsquentesten umgesetzt. Jeder kann sich unterschiedliche Formattierungsregeln aneignen, aber es ist besser, wenn das nicht notwendig ist und jeder den gleichen Stil verwendet. Dadurch kann viel Zeit gespart werden, da dieses Thema nicht jedes mal neu diskutiert werden muss. Das Problem an der Stelle ist, wie man dieses Ziel ohne umfangreichen und komplizierten Style Guide umsetzen kann.

Mit Go fahren wir einen unüblichen Ansatz und lassen die Maschine über die Formatierung bestimmen. Das `gofmt` Programm (auch über den Befehl `go fmt` zugänglich) liest das Go Programm ein und formatiert den Quellcode in einem Standart-Stil. Dabei werden Einrückungen gesetzt und eine vertikale Ausrichtung vorgenommen. Wenn notwendig werden dabei auch Kommentare neu formattiert. Wenn unklar ist, wie gewisse Passagen formatiert werden sollen, kann einfach `gofmt` ausgeführt werden. Wenn das Ergebnis nicht korrekt aussieht, sollte das Programm angepasst werden (oder ein Bug zu `gofmt` gemeldet werden). An der Stelle sollte jedoch kein Workaround erstellt werden.

Hier ein kleines Beispiel, welches die Funktionsweise zeigen soll. An der Stelle ist es nicht notwendig die Kommentare zu den Feldern eines Struct auszurichten. `gofmt` erledigt das automatisch. Aus dem folgenden Code

```go
type T struct {
    name string // name of the object
    value int // its value
}
```
`gofmt`richtet hier die einzelnen Spalten aus:

```go
type T struct {
    name    string // name of the object
    value   int    // its value
}
```

Der komplette Go Code der Standard Packete wurde mit `gofmt` formatiert.

Ein paar Details sind jedoch weiterhin offen:

* Einrückung: Wir verwenden Tabs für Einrückungen und `gofmt` erzeugt diese im Standardfall. Spaces sollen nur verwendet werden, wenn diese wirklich notwendig ist
* Zeilenlänge: Go hat keine Einschränkung bezüglich der Zeilenlänge. Aber auch keine Angst vor unendlich langen Zeilen. Wenn eine Zeile gefühlt zu lange wird, dann kann diese einfach umgebrochen werden und mit einem extra Tab eingerückt werden.
* Klammern: Go verwendet weniger Klammern als C oder Java. Die Kontroll Struktueren (`if`, `for`, `switch`) benötigen laut Syntax keine Klammern. Auch die Rangordnung der Operatoren ist kürzer und klarer. Das Beispiel

```go
x<<8 + y<<16
```
berechnet sich, wie es die Leerzeichen definieren.
