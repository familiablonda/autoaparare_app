# Schimbarea iconiței aplicației Flutter pentru toate platformele

Acest ghid explică cum să configurezi iconițele aplicației pentru Android, iOS, macOS și Windows folosind pachetul `flutter_launcher_icons`.

## 1. Cerințe preliminare

### 1.1 Imagine sursă
- Format: PNG (recomandat)
- Dimensiune: 1024x1024 pixeli
- Aspect: Pătrat (1:1)
- Transparență: Acceptată
- Calitate: Imagine clară, vizibilă la dimensiuni mici

### 1.2 Structura folderelor
```
your_app/
├── assets/
│   └── icon/
│       ├── app_icon.png
│       └── app_icon_foreground.png (opțional, pentru Android adaptive icon)
├── ios/
├── android/
├── macos/
│   └── Runner/
│       └── Assets.xcassets/
│           └── AppIcon.appiconset/
└── windows/
```

## 2. Configurare

### 2.1 Adăugare dependență
Adaugă `flutter_launcher_icons` în `pubspec.yaml` sub `dev_dependencies`:

```yaml
dev_dependencies:
  flutter_launcher_icons: ^0.13.1
```

### 2.2 Configurare completă
Adaugă următoarea configurație în `pubspec.yaml`:

```yaml
flutter_launcher_icons:
  # Configurare generală
  image_path: "assets/icon/app_icon.png"
  
  # Android
  android: true
  min_sdk_android: 21
  adaptive_icon_background: "#ffffff"
  adaptive_icon_foreground: "assets/icon/app_icon_foreground.png"
  
  # iOS
  ios: true
  remove_alpha_ios: true
  
  # macOS
  macos:
    generate: true
    image_path: "assets/icon/app_icon.png"
    sizes:
      - 16
      - 32
      - 64
      - 128
      - 256
      - 512
      - 1024
    
  # Windows
  windows:
    generate: true
    image_path: "assets/icon/app_icon.png"
    icon_size: 48 # Dimensiunea implicită
```

## 3. Pregătire pentru fiecare platformă

### 3.1 Android
- Asigură-te că `android/app/src/main/res` există
- Pentru Android 8+ (API 26+), pregătește și o imagine pentru foreground

### 3.2 iOS
- Verifică că `ios/Runner` există
- Pentru App Store, asigură-te că iconița nu are transparență

### 3.3 macOS
1. Creează structura necesară:
```bash
mkdir -p macos/Runner/Assets.xcassets/AppIcon.appiconset
```

2. Creează `Contents.json`:
```bash
touch macos/Runner/Assets.xcassets/AppIcon.appiconset/Contents.json
```

3. Adaugă configurația pentru `Contents.json`:
```json
{
  "images" : [
    {
      "size" : "16x16",
      "idiom" : "mac",
      "filename" : "app_icon_16.png",
      "scale" : "1x"
    },
    {
      "size" : "16x16",
      "idiom" : "mac",
      "filename" : "app_icon_32.png",
      "scale" : "2x"
    },
    {
      "size" : "32x32",
      "idiom" : "mac",
      "filename" : "app_icon_32.png",
      "scale" : "1x"
    },
    {
      "size" : "32x32",
      "idiom" : "mac",
      "filename" : "app_icon_64.png",
      "scale" : "2x"
    },
    {
      "size" : "128x128",
      "idiom" : "mac",
      "filename" : "app_icon_128.png",
      "scale" : "1x"
    },
    {
      "size" : "128x128",
      "idiom" : "mac",
      "filename" : "app_icon_256.png",
      "scale" : "2x"
    },
    {
      "size" : "256x256",
      "idiom" : "mac",
      "filename" : "app_icon_256.png",
      "scale" : "1x"
    },
    {
      "size" : "256x256",
      "idiom" : "mac",
      "filename" : "app_icon_512.png",
      "scale" : "2x"
    },
    {
      "size" : "512x512",
      "idiom" : "mac",
      "filename" : "app_icon_512.png",
      "scale" : "1x"
    },
    {
      "size" : "512x512",
      "idiom" : "mac",
      "filename" : "app_icon_1024.png",
      "scale" : "2x"
    }
  ],
  "info" : {
    "version" : 1,
    "author" : "xcode"
  }
}
```

### 3.4 Windows
- Asigură-te că folderul `windows` există în proiect
- Iconița va fi generată în format `.ico`

## 4. Generare iconițe

Rulează următoarele comenzi în ordine:

```bash
# Curăță proiectul
flutter clean

# Actualizează dependențele
flutter pub get

# Generează iconițele
flutter pub run flutter_launcher_icons

# Verifică generarea pentru fiecare platformă
flutter run -d android
flutter run -d ios
flutter run -d macos
flutter run -d windows
```

## 5. Verificare după instalare

### 5.1 Android
- Verifică în `android/app/src/main/res/mipmap-*`
- Verifică iconița adaptivă în `android/app/src/main/res/mipmap-anydpi-v26`

### 5.2 iOS
- Verifică în `ios/Runner/Assets.xcassets/AppIcon.appiconset`

### 5.3 macOS
- Verifică în `macos/Runner/Assets.xcassets/AppIcon.appiconset`
- Verifică generarea `AppIcon.icns`

### 5.4 Windows
- Verifică în `windows/runner/resources`
- Verifică fișierul `.ico` generat

## 6. Depanare

### 6.1 Erori comune și soluții

#### Android
- Eroare: "Adaptive icon requires background and foreground"
  ```bash
  # Adaugă în pubspec.yaml
  adaptive_icon_background: "#ffffff"
  adaptive_icon_foreground: "path/to/foreground.png"
  ```

#### iOS/macOS
- Eroare: "No podspec found"
  ```bash
  cd ios # sau cd macos
  pod install
  cd ..
  ```

- Eroare: "AppIcon.appiconset missing"
  ```bash
  # Recreează structura folderelor și Contents.json
  ```

#### Windows
- Eroare: "Icon size must be multiple of 8"
  ```yaml
  # Specifică dimensiunea în pubspec.yaml
  windows:
    icon_size: 48
  ```

### 6.2 Curățare completă
```bash
# Șterge toate build-urile
rm -rf build
flutter clean

# Pentru iOS/macOS
cd ios # sau cd macos
pod deintegrate
pod install
cd ..

# Regenerează
flutter pub get
flutter pub run flutter_launcher_icons
```

## 7. Note importante

1. Dezinstalează aplicația înainte de a testa noua iconiță
2. Verifică iconița pe diferite dispozitive și versiuni OS
3. Pentru Android 8+, folosește întotdeauna iconițe adaptive
4. Pentru iOS/macOS, evită transparența
5. Pentru Windows, asigură-te că iconița arată bine pe fundal închis

## 8. Recomandări pentru design

1. Păstrează designul simplu și clar
2. Testează vizibilitatea la dimensiuni mici
3. Verifică cum arată în modul întunecat
4. Respectă ghidurile de design pentru fiecare platformă
5. Folosește culori care contrastează bine