
# Proje Dokümantasyonu

Bu belgede, Java dilinde geliştirilen "Real-Time Grammar-Based Syntax Highlighter
with GUI" projesinin çalışma mantığı, genel yapısı ve işleyiş süreci ayrıntılı şekilde açıklanmıştır.

## <br>1. Programlama Dili ve Gramer Seçimi

### Seçilen Dil: 
Basitleştirilmiş C benzeri programlama dili

### Gramer Özellikleri:
- Anahtar kelimeler: `int`, `if`, `else`, `while`, `return`
- Değişken tanımlama ve atama
- Aritmetik ifadeler
- Kontrol yapıları (if-else, while döngüsü)
- Blok yapıları (`{` `}`)

### Gramer Kuralları:

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

## <br>2. Sözdizimi Analizi Süreci

Sözdizimi analizi iki aşamada gerçekleşir:

1. **Leksikal Analiz:** Kaynak kodu tokenlara böler
2. **Sözdizimsel Analiz:** Tokenları gramer kurallarına göre analiz eder

## <br>3. Leksikal Analiz Detayları

### Seçilen Yöntem: 
State Diagram & Program Implementation

### Token Tipleri:
- **KEYWORD:** Anahtar kelimeler
- **IDENTIFIER:** Değişken isimleri
- **NUMBER:** Sayılar
- **OPERATOR:** İşleçler (`+`, `-`, `=`, `<`, `>` vb.)
- **SYMBOL:** Semboller (`{`, `}`, `;`, `(`, `)` vb.)
- **WHITESPACE:** Boşluklar
- **UNKNOWN:** Tanınmayan karakterler

### Çalışma Prensibi:
1. Regex pattern ile tüm token tiplerini tanımla
2. Matcher ile metni tara
3. Her bulunan lexeme için tip belirle
4. Token listesi oluştur

## <br>4. Ayrıştırma (Parsing) Metodolojisi

### Seçilen Yöntem: 
Top-Down Parser (Yukarıdan Aşağıya)

### Parser Özellikleri:
- Recursive descent parser
- Backtracking ile hata kurtarma
- LL(1) gramer yapısı

### Ana Fonksiyonlar:
- `parseProgram()`: Ana program yapısını ayrıştırır
- `parseStatement()`: İfade tiplerini belirler
- `parseExpression()`: Matematiksel ifadeleri işler
- `parseBlock()`: Blok yapılarını kontrol eder

## <br>5. Vurgulama Şeması

### Vurgulama Stratejisi:
- Her token tipi için farklı renk ve stil
- Gerçek zamanlı güncelleme
- Swing StyledDocument kullanımı

### Renk Şeması:
- **KEYWORD:** Mavi, kalın
- **IDENTIFIER:** Siyah, normal
- **NUMBER:** Mor, normal
- **OPERATOR:** Kırmızı, kalın
- **SYMBOL:** Koyu gri, normal
- **UNKNOWN:** Gri, normal

### Vurgulama Algoritması:
1. Metin değişikliğinde tokenize et
2. Her token için pozisyon hesapla
3. StyledDocument ile renklendirme uygula
4. Cursor pozisyonunu koru

## <br>6. GUI Implementasyonu

### Kullanılan Teknoloji: 
Java Swing

### Ana Bileşenler:
- **JTextPane:** Kod editörü
- **StyledDocument:** Metin stillemesi
- **DocumentListener:** Gerçek zamanlı güncelleme
- **Timer:** Performans optimizasyonu

## <br>7. Gerçek Zamanlı Fonksiyonalite

### Çalışma Prensibi:
1. Kullanıcı yazdığında DocumentListener devreye girer
2. Metin tokenize edilir ve ayrıştırılır
3. Vurgulama güncellenir
4. Cursor pozisyonu korunur

### Teknik Detaylar:
- SwingUtilities ile thread safety
- Exception handling ile hata yönetimi
- Memory-based storage

## Sonuç

Bu proje bir sözdizimi vurgulayıcı oluşturur. Top-down parsing ve regex-based lexical analysis kullanarak, gerçek zamanlı C-benzeri kod vurgulama sağlar. Java Swing ile kullanıcı dostu bir arayüz sunar.

## <br>Projenin Bileşenleri

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

---

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

---

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

---

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

---

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

---


## Özet

| Bileşen           | Açıklama                                     |
|-------------------|----------------------------------------------|
| Lexer             | Kodun token’lara ayrıştırılmasını sağlar     |
| Parser            | Token’ların sözdizimsel analizini yapar      |
| GUI (Swing)       | Gerçek zamanlı vurgulama sağlar              |
| Token & TokenType | Temel veri yapıları                          |


