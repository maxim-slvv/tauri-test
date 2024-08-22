То что получилось на выходе и можно запустить лежит в папке ```READY_FOR_RUN```:
  - ```app.apk``` (уже подписанный)
  - ```app.exe```


Установка и запуск Windows:

1. RUST и Cargo
```https://v2.tauri.app/start/prerequisites/#rust```
  >проверить что все ок
```rustc --version```
```cargo --version```

  >что бы PATH обновился - перезагрузить терминал

2. Microsoft C++ Build Tools и Install WebView2 - для разрабоки по windows.
```https://v2.tauri.app/start/prerequisites/#windows```

3. Сначала собрать react приложение(html, css, js).
   ```npm run dev```

4. Собрать из html, css, js - Windows приложение.
   ```npm run tauri dev```

5. Файл .exe будет в 
```src-tauri/target/release/app.exe``` 


Установка и запуск Android: 

1. Установить переменную окружения для SDK
  - Имя переменной: ```ANDROID_HOME```
  - Значение:
  ```На Windows: C:\Users\YourUsername\AppData\Local\Android\Sdk```
  ```На macOS: /Users/YourUsername/Library/Android/sdk```
  ```На Linux: /home/YourUsername/Android/Sdk```
  > например (C:\Users\Diluc\AppData\Local\Android\Sdk) 

2. Установить Android NDK(v.27.0.12077973) в Android Studio и создать переменную ```NDK_HOME``` 
  > Полная инструкция https://developer.android.com/studio/projects/install-ndk#specific-version 
  - Откройте Android Studio (у меня Android Studio Koala) https://developer.android.com/studio 
  - Перейдите в SDK Manager(что бы найти этот пункт - необходимо создать любой проект и уже там будут более расширенные настройки в отличие от экрана приветствия)
  - Во вкладке "SDK Tools" найдите и установите "NDK (Side by side)" нужной версии.
  - Теперь в папке по пути ```ANDROID_HOME``` - появилась папка ndk - нужно скопировать путь на нее и установить в переменную ```NDK_HOME``` 
  > например (C:\Users\Diluc\AppData\Local\Android\Sdk\ndk\27.0.12077973) 

3. Установить переменную ```JAVA_HOME```
  - https://v2.tauri.app/start/prerequisites/#android
  - C:\Program Files\Android\Android Studio\jbr

4. Инициализация android: если нету папки src-tauri/gen/
  - ```npm run tauri android init```
  > появятся файлы которые не отслеживаются git

5. Запуск проекта:
  - ввести ```npm run tauri android dev``` 
  - предложит несколько эмуляторов - у меня был ```Medium_Phone_API_35``` 

6. Сборка APK:
  - Установить переменную ANDROID_BUILD_TOOLS - что бы потом ей подписать приложение 
  > У меня C:\Users\Diluc\AppData\Local\Android\Sdk\build-tools\35.0.0 
  - включить режим разработчика: 
  > например для винды 10+: https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development 

  - Запуск сборки: ```npm run tauri android build``` 
  - Создастся файл и будет указан путь на него в cli 
  > например ...tauri-test\src-tauri\gen\android\app\build\outputs\apk\universal\release\ 
  - Неподписанные файлы называются ```app-universal-release-unsigned.apk``` 
  > неподписанные файлы - не устанавливаются - для этого нужно создать хранилище ключей и подписать им приложение...
  - Создать хранилище ключей: 
  https://developer.android.com/studio/publish/app-signing#generate-key 
  - Подписать приложение: 
  https://developer.android.com/studio/publish/app-signing#sign_release 

  просто заметка: поместил файл с ключами .jks и .apk в одну папку и перешел в нее в терминале 
  и запустил команду (в гайде подсказка как это сделать) 
  ```"C:\Users\Diluc\AppData\Local\Android\Sdk\build-tools\35.0.0\apksigner.bat" sign --ks anroid-test-key.jks --out test-app-release-signed.apk test-app-universal-release-unsigned.apk```


далее тут стандартный readme:
------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------


# React + TypeScript + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react/README.md) uses [Babel](https://babeljs.io/) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh

## Expanding the ESLint configuration

If you are developing a production application, we recommend updating the configuration to enable type aware lint rules:

- Configure the top-level `parserOptions` property like this:

```js
export default tseslint.config({
  languageOptions: {
    // other options...
    parserOptions: {
      project: ['./tsconfig.node.json', './tsconfig.app.json'],
      tsconfigRootDir: import.meta.dirname,
    },
  },
})
```

- Replace `tseslint.configs.recommended` to `tseslint.configs.recommendedTypeChecked` or `tseslint.configs.strictTypeChecked`
- Optionally add `...tseslint.configs.stylisticTypeChecked`
- Install [eslint-plugin-react](https://github.com/jsx-eslint/eslint-plugin-react) and update the config:

```js
// eslint.config.js
import react from 'eslint-plugin-react'

export default tseslint.config({
  // Set the react version
  settings: { react: { version: '18.3' } },
  plugins: {
    // Add the react plugin
    react,
  },
  rules: {
    // other rules...
    // Enable its recommended rules
    ...react.configs.recommended.rules,
    ...react.configs['jsx-runtime'].rules,
  },
})
```
