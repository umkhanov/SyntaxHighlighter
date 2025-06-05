# Real-Time Grammar-Based Syntax Highlighter

Gerçek zamanlı sözdizimi vurgulayıcı - C benzeri programlama dili için geliştirilmiş GUI tabanlı syntax highlighter.

- **Dokümantasyon** - [Dokümantasyonu inceleyin](https://youtu.be/92CO8lLTZLs)
- **Demo Video** - [Uygulamanın çalışır halini izleyin](https://youtu.be/92CO8lLTZLs)
- **Teknik Makale** - [Detaylı implementasyon açıklamaları](https://medium.com/@umhanov04/ger%C3%A7ek-zamanl%C4%B1-grammar-tabanl%C4%B1-syntax-highlighter-geli%C5%9Ftirme-s%C3%BCreci-aa4c2a301d6b)

## Başlangıç

### Projeyi Çalıştırma

```bash
javac *.java
java SyntaxHighlighterGUI
```

### Sistem Gereksinimleri
- Java 8 veya üzeri
- Swing GUI desteği

## Proje Yapısı

| Dosya | Açıklama |
|-------|----------|
| `Lexer.java` | Leksikal analiz motoru - metni token'lara böler |
| `Parser.java` | Top-down parser - grammar kurallarını kontrol eder |
| `Token.java` | Token veri yapısı |
| `TokenType.java` | Token türleri enum'u |
| `SyntaxHighlighterGUI.java` | Ana GUI uygulaması |

## Özellikler

- **Gerçek zamanlı vurgulama**: Yazdığınız anda sözdizimi renklendirme
- **7 token türü desteği**: Anahtar kelime, tanımlayıcı, sayı, operatör, sembol, boşluk, bilinmeyen
- **C benzeri dil grammar'ı**: int, if, else, while, return anahtar kelimeleri
- **Top-down parsing**: LL(1) grammar yapısı
- **Kullanıcı dostu arayüz**: Swing tabanlı modern GUI

## Desteklenen Grammar

```
Program → StatementList
StatementList → Statement StatementList | ε
Statement → Declaration | Assignment | If | While | Block
Declaration → int IDENTIFIER ;
Assignment → IDENTIFIER = Expression ;
If → if ( Expression ) Block [else Block]
While → while ( Expression ) Block
Block → { StatementList }
Expression → Term [Operator Term]*
Term → Factor
Factor → IDENTIFIER | NUMBER | ( Expression )
```

## Token Türleri ve Renkleri

- **KEYWORD** - Mavi: `int`, `if`, `else`, `while`, `return`
- **IDENTIFIER** - Siyah: Değişken isimleri
- **NUMBER** - Mor: Sayısal değerler
- **OPERATOR** - Kırmızı: `=`, `+`, `-`, `*`, `/`, `>`, `<`, `==`, `!=`, `<=`, `>=`
- **SYMBOL** - Koyu gri: `{`, `}`, `(`, `)`, `;`
- **UNKNOWN** - Gri: Tanınmayan karakterler


## Teknik Detaylar

### Leksikal Analiz
- **Yöntem**: State Diagram & Program Implementation
- **Teknoloji**: Java Regex Pattern Matching
- **Algoritma**: Finite State Automaton

### Parser
- **Yöntem**: Top-Down (Recursive Descent)
- **Tip**: LL(1) Grammar
- **Özellik**: Backtracking ile hata kurtarma

### GUI
- **Framework**: Java Swing
- **Bileşenler**: JTextPane, StyledDocument, DocumentListener
- **Performans**: Timer tabanlı güncelleme optimizasyonu (100ms gecikme)

## Örnek Kullanım

```c
int sayi = 42;
if (sayi > 10) {
    sayi = sayi * 2;
    while (sayi < 100) {
        sayi = sayi + 5;
    }
}
return sayi;
```

## Mimari

```
SyntaxHighlighterGUI
    ├── Lexer (Tokenization)
    ├── Parser (Grammar Validation)
    └── Real-time Highlighting
```

*Bu proje, compiler design prensiplerini kullanarak geliştirilmiş eğitim amaçlı bir syntax highlighter'dır.*
