
# Gerçek Zamanlı Sözdizimi Vurgulayıcı - Belgeler

Bu belgede, Java dilinde geliştirilen "Gerçek Zamanlı Sözdizimi Vurgulayıcı (Syntax Highlighter)" projesinin tüm bileşenleri ayrıntılı şekilde açıklanmıştır. Projede sözdizimi analizi (lexer & parser) ve grafik kullanıcı arayüzü (GUI) bileşenleri bulunmaktadır.



## Projenin Bileşenleri

### 1. `TokenType.java` 

Token türlerini temsil eden `enum` sınıfıdır. Her token, sözdizimsel rolüne göre bir türe sahiptir:

```java
public enum TokenType {
    KEYWORD,
    IDENTIFIER,
    NUMBER,
    OPERATOR,
    SYMBOL,
    WHITESPACE,
    UNKNOWN
}
```
<br>
 

### 2. `Token.java`

Bir token’ı temsil eden sınıftır.

#### Alanlar:
- `value`: Token’ın metinsel içeriği
- `type`: Token’ın tipi (`TokenType` enum değeri)

#### Metotlar:
```java
public String getValue()
public TokenType getType()
public String toString()
```
<br>
 

### 3. `Lexer.java`

Kod metnini `Token` nesnelerine ayıran sözcük çözümleyicisidir (lexer).

#### Öne Çıkan Noktalar:
- Anahtar kelimeler: `int`, `if`, `else`, `while`, `return`
- Tanıyabildiği token tipleri: boşluk, anahtar kelime, tanımlayıcı, sayı, operatör, sembol

#### Ana Metot:
```java
public static List<Token> tokenize(String code)
```

#### Kullanım:
```java
String code = "int x = 5;";
List<Token> tokens = Lexer.tokenize(code);
```
<br>
 

### 4. `Parser.java`

Token listesini kontrol ederek kodun sözdizimsel olarak geçerli olup olmadığını kontrol eder.

#### Temel Kurallar:
- Değişken tanımı: `int x;`
- Atama: `x = 10;`
- `if`, `while` blokları
- Blok: `{ ... }`
- İfade: matematiksel ya da karşılaştırmalı ifadeler

#### Örnek:
```java
String code = "int x = 10; if (x > 5) { x = x + 1; }";
List<Token> tokens = Lexer.tokenize(code);
Parser parser = new Parser(tokens);

if (parser.parseProgram()) {
    System.out.println("Ayrıştırma başarılı!");
}
```
<br>
 

### 5. `SyntaxHighlighterGUI.java`

Swing kütüphanesi ile gerçek zamanlı sözdizimi vurgulaması yapan grafik arayüz.

#### Özellikler:
- `JTextPane` kullanılarak metin girilebilir arayüz
- `StyledDocument` ile her `TokenType` için renkli vurgulama
- `DocumentListener` sayesinde her değişiklik sonrası vurgulama yapılır
- Vurgulama sırasında özyinelemeyi engelleyen `isHighlighting` bayrağı

#### Stil Renkleri:
| Token Türü    | Renk        |
|---------------|-------------|
| Anahtar Kelime| Mavi        |
| Tanımlayıcı   | Siyah       |
| Sayı          | Mor         |
| Operatör      | Kırmızı     |
| Sembol        | Koyu Gri    |
| Bilinmeyen    | Gri         |

#### Başlatma:
```java
public static void main(String[] args) {
    SwingUtilities.invokeLater(() -> new SyntaxHighlighterGUI());
}
```
<br>
 

## Örnek Kod Girişi

Aşağıdaki örnek GUI üzerinde test edilmiştir:

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
<br>

## Özet

| Bileşen           | Açıklama                                     |
|-------------------|----------------------------------------------|
| Lexer             | Kodun token’lara ayrıştırılmasını sağlar     |
| Parser            | Token’ların sözdizimsel analizini yapar      |
| GUI (Swing)       | Gerçek zamanlı vurgulama sağlar              |
| Token & TokenType | Temel veri yapıları                          |


## Notlar

- Kodun tamamı Java dilinde yazılmıştır.
- GUI, `JTextPane` üzerinden çalışır ve her değişiklikte `Lexer` kullanılarak yeniden vurgulama yapılır.
