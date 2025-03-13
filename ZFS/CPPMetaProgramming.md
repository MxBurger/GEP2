## Templates allgemein

- Templates dienen als Blaupausen für Familien von Klassen und Funktionens
- Sie ermöglichen generische Algorithmen und Datenstrukturen
- Sie ergänzen die Subklassen-Polymorphie (virtuelle Funktionen) durch parametrische Polymorphie in C++
- Sie ermöglichen parametrisierte Komponenten 
- Sie ermöglichen Berechnungen zur Compile-Zeit (statische Metaprogrammierung)

> Diese Eigenschaften machen Templates zu einem leistungsstarken Werkzeug für generische und flexible Programmierung in C++, wodurch Code wiederverwendbar und typsicher gestaltet werden kann.

## Function Templates vs. Class Templates

### Function Template

Beispiel:
```cpp
template <class T>
void swap(T& a, T& b) {
T c = a;
a = b;
b = c;
}
```

Die Funktion kann mittels impliziter (automatischer) Typausprägung aufgerufen werden:
```cpp
int i1 = 1, i2 = 2;
swap(i1,i2);
```

oder die Typ-Qualifizierung explizit angegeben werden:
```cpp
int i1 = 1, i2 = 2;
swap<int>(i1,i2);
```

### Class Template
Bei Class Templates muss der Datentyp immer explizit angegeben werden.

```cpp
template <class T>
class Array {
public:
    T data[10];
};
```
Verwendung:
```cpp
Array<int> intArray;
Array<double> doubleArray;
```

### Template Specialization

Allgemeine Definition:
```cpp
template <class T, int size>
class Array {
public:
T& operator[](int i);
    // ...
    T data[size];
};
```

Es gibt zwei Arten der Spezialisierung von Templates in C++:

#### Vollständige Spezialisierung

Eine spezfische Implementierung wird für einen bestimmten Datentyp definiert.

Eine Variante einer vollständigen Spezialisierung:
```cpp
// full specialization
template <>
class Array<bool, 8> {
    // ...
    // memory for 8 bits
    char data;
};
```

*Diese spezialisierte Implementierung benützt einen `char` als interne Datenstruktur.*

### Teilweise Spezialisierung
Es wird nur ein Teil der Template-Parameter spezialisiert, während andere weiterhin generisch bleiben.

```cpp
template <int size>
class Array<bool, size> {
    // ...
    // memory for 'size' bits
    char data[(sizeof(char) + size - 1) / sizeof(char)];
};
```
Verwendung:
```cpp
Array<int,10> a1;
Array<bool,8> a1;
Array<bool,16> a3;
```

Weitere Beispiele:
```cpp
template <class T1, class T2>
struct X {
    void hello() {
        cout << "general" << endl;
    }
};

// full spezialisation
template <>
struct X<int, double> {
    void hello() {
        cout << "fully spec." << endl;
    }
};

// partial spezialisation
template <class T2>
struct X<int,T2> {
    void hello() {
    cout << "partially spec. (int,T2)" << endl;
    }
};

// partial spezialisation
template <class T>
struct X<T,T> {
    void hello() {
        cout << "partially spec. (T,T)" << endl;
    }
};

// ...
X<char,char>().hello();
X<int,bool>().hello();
X<int,double>().hello();
```