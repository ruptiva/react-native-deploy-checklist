## React Native - Deploy Checklist

**Ícones**
- Imagem base com resolução de 1024x1024 pixels, JPG.
- Gerar usando [app-icon](https://www.npmjs.com/package/app-icon)

**Splashscreen**
- [How to Add a Splash Screen to a React Native App (iOS and Android)](https://medium.com/handlebar-labs/how-to-add-a-splash-screen-to-a-react-native-app-ios-and-android-30a3cec835ae)


### iOS
**Archiving de projetos**
- Gerar IPAs universais (para iPhone e iPad) para evitar problemas de __auto scaling__ ao usar o app em tablets.

**Push Notification Entitlement**
- Para evitar o erro __Missing  Push Notification Entitlement__, basta adicionar pelo XCode, Clicando no arquivo
`.xcodeproj` ou `.xcworkspace` do projetoe marcando __ON__ na opção __Push Notifications__, dentro da aba __Capabilities__.

## Android

**Permissões indesejadas**
- [Fixing React Native Android Permissions](https://medium.com/@applification/fixing-react-native-android-permissions-9e78996e9865)

**Fontes**
- O Android tem problemas com fontes cujo arquivos tenham letras maiúsculas no nome. Para resolver isso,
renomeie os arquivos para minúsculas, adicionando __underline__ nos espaços. Lembre-se de importar o arquivo
correto caso as fontes do iOS não sejam renomeadas.

```js
   //iOS font: My Awesome Font Bold.otf
   //Android font: my_awesome_font_bold.otf
   import { Platform } from 'react-native'

   export const FONT_BOLD = Platform.OS === 'ios' ? 'My Awesome Font Bold' : 'my_awesome_font_bold'
```

**Alterar build tools**
- Faça a seguinte alteração dentro do arquivo `build.gradle (Module: app)`:

```gradle
defaultConfig {
        targetSdkVersion 26 //Altere o valor que está aqui para 26
    }
```

**Reduzir tamanho da APK**
- Faça a seguinte alteração dentro do arquivo `build.gradle (Module: app)`:

```gradle
 defaultConfig {
        ndk {
            abiFilters "armeabi-v7a" //Remova "x86" que está aqui
        }
    }

splits {
        abi {
            reset()
            enable enableSeparateBuildPerCPUArchitecture
            universalApk false 
            include "armeabi-v7a" //Remova "x86" que está aqui
        }
    }
```

**Erro ao gerar APK usando Gradle 4.4**
- Gere um novo bundle pelo terminal (Adicione a linha a seguir nos `scripts` do `package.json` pra reuso)
`"bundle:android": "mkdir -p android/app/src/main/assets && rm -rf android/app/build && react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res"`

- Após o bundle ser criado, remova todas as imagens das seguintes pastas:
* `android/app/src/main/res/drawable-mdpi`
* `android/app/src/main/res/drawable-hdpi`
* `android/app/src/main/res/drawable-xhdpi`
* `android/app/src/main/res/drawable-xxhdpi`
* `android/app/src/main/res/drawable-xxxhdpi`

- Navegue para a pasta `android` do seu projeto e execute `./gradlew assembleRelease`
