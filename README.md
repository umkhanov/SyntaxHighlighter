# Real-Time Grammar-Based Syntax Highlighter

Gerçek zamanlı sözdizimi vurgulayıcı - C benzeri programlama dili için geliştirilmiş GUI tabanlı syntax highlighter.

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
| `Lexer.java`  Leksikal analiz motoru - metni token'lara böler |
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
Term → IDENTIFIER | NUMBER | ( Expression )
```

## Token Türleri ve Renkleri

- **KEYWORD** - Mavi: `int`, `if`, `else`, `while`, `return`
- **IDENTIFIER** - Siyah: Değişken isimleri
- **NUMBER** - Mor: Sayısal değerler
- **OPERATOR** - Kırmızı: `=`, `+`, `-`, `*`, `/`, `>`, `<`, `==`, vb.
- **SYMBOL** - Koyu gri: `{`, `}`, `(`, `)`, `;`
- **UNKNOWN** - Açık gri: Tanınmayan karakterler

## Demo ve Dokümantasyon

- **[Demo Video](https://youtu.be/92CO8lLTZLs)** - Uygulamanın çalışır halini izleyin
- **[Teknik Makale](YOUR_MEDIUM_LINK_HERE)** - Detaylı implementasyon açıklamaları

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
- **Performans**: Timer tabanlı güncelleme optimizasyonu

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

*Bu proje, compiler design prensiplerini kullanarak geliştirilmiş eğitim amaçlı bir syntax highlighter'dır.*
