# Node.js

## Was ist Node.js
* Node.js ist eine Javascript-Laufzeitumgebung
* Basiert auf V8-JavaScript-Engine von Chrome 
* Mit Node.js lässt sich Javascript außerhalb von Browser ausführen.

## Typen
* String, Number, Boolean, Undefined, Null, RegExp
* Alles andere ist ein Objekt
* Automatisches typecasting
* == führt automatisch ein typecast aus 2 == "2" -> true
## Buffer

# Events

# DB

# ORM

## Module 
* Anders als bei Javascript wird alles lokal gehalten. 
* Mit exports.name = object können Objekte in global verwendet werden.
* Mit let obj = require('./modul/toImport.js) lassen sich andere Module(/Objete aus anderen Modulen) importieren
* Module lassen sich über npm installieren

## Callback Hell
* lässt sich vermeiden durch:
    * vermeiden von anonymen funktionen
    * Fehlerbehandlungen von einzelnen "Fehlern", die in jeder Callback auftreten können
    * Wiederverwendare Funktionen/Module erstellen (Code aufteilen)
    * Promises
    * Generators
    * Async functions
 http://callbackhell.com/   
## Debugging
* core debugger:
    * mit dem keyword debugger wird ein breakpoint im code gesetzt
    * weitere breakpoints können mit 
        * setBreakpoint(), sb() - Set breakpoint on current line
        * setBreakpoint(line), sb(line) - Set breakpoint on specific line
     gesetzt werden
    * cont, c - Continue execution
    * next, n - Step next
    * step, s - Step in
    * out, o - Step out
* node-inspector (V8 inspector)
    * mit node-debug filename.js wird die app mit debugger gestartet
## Tests
* mocha
* jest
* istanbul
* chai
## Deployment

## Node.js Design Fundamentals
