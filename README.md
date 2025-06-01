# Real-Time Grammar-Based Syntax Highlighter Projesi Dokümantasyonu

## Projeyi Çalıştırma Talimatı
Projeyi test etmek için SyntaxHighlighterGUI.java dosyasını çalıştırın!

<pre>bash
javac *.java
java SyntaxHighlighterGUI</pre> 
## <br>Demo Video
Demo Video linki : https://youtu.be/92CO8lLTZLs <br><br>


## 1. Programlama Dili ve Gramer Seçimi

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

### Neden Bu Yöntem:
- Regex kullanarak basit ve etkili
- Programlı kontrol ile esnek token tipleri
- Öğrenci seviyesi için anlaşılır

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

### Neden Top-Down:
- Gramer yapısını doğal olarak takip eder
- Recursive descent parsing ile basit implementasyon
- Sol-faktorizasyon gerektirmez

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

### Özellikler:
- Gerçek zamanlı sözdizimi vurgulama
- Kaydırma çubuğu desteği
- Durum çubuğu
- Örnek kod yükleme

### Performans Optimizasyonları:
- 100ms gecikme ile vurgulama
- Özyineleme kontrolü
- Timer ile fazla güncelleme engelleme

## <br>7. Gerçek Zamanlı Fonksiyonalite

### Çalışma Prensibi:
1. Kullanıcı yazdığında DocumentListener devreye girer
2. Timer 100ms bekler (çok sık güncellemeyi önler)
3. Metin tokenize edilir ve ayrıştırılır
4. Vurgulama güncellenir
5. Cursor pozisyonu korunur

### Teknik Detaylar:
- SwingUtilities ile thread safety
- Exception handling ile hata yönetimi
- Memory-based storage (localStorage yok)

## Sonuç

Bu proje, basit ama etkili bir sözdizimi vurgulayıcı oluşturur. Top-down parsing ve regex-based lexical analysis kullanarak, gerçek zamanlı C-benzeri kod vurgulama sağlar. Java Swing ile kullanıcı dostu bir arayüz sunar.
