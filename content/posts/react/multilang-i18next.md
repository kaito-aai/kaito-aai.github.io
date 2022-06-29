---
title: "React: i18nextで多言語対応"
date: 2022-06-29T17:30:11+09:00
draft: false
---

Reactで多言語対応をしたときの手順メモ  
言語はブラウザに設定されたものを利用する前提  

使うもの
- https://www.i18next.com/
- https://github.com/i18next/i18next-browser-languageDetector/tree/master
    - i18next-browser-languagedetectorでブラウザの言語を扱えるようになる  


## 1. インストール

> npm install react-i18next i18next i18next-browser-languagedetector --save

## 2. 設定
i18n用ファイルを用意し、多言語用リソースを扱うための諸々を設定  
以下のファイルを追加

i18n.ts  
``` ts
import i18n from "i18next";
import LanguageDetector from 'i18next-browser-languagedetector'; 
import { initReactI18next } from "react-i18next";

const resources = {
    ja: {
        translation: {
            "Title": "ホームページ",
        }
    },
    en: {
        translation: {
            "Title": "Home Page",
        }
    }
};

i18n.use(initReactI18next)
    .use(LanguageDetector)
    .init({
        // lng: 'en' <-　LanguageDetectorが機能しないのでこの設定は削除
        resources,
        fallbackLng: 'en',
        supportedLngs: ['en', 'ja'],
        interpolation: {
            escapeValue: false
        },
        detection: {
            // navigatorの優先度を上げる
            order: ['navigator'],
            // デフォルトの設定だとlocalStorage, cookieにcacheするようになっている。
            // ブラウザの設定を参照したいので削除
            caches: []
        }
    })

export default i18n;
```

## 3.多言語リソースの使用

App.tsx
``` ts
import { useTranslation } from 'react-i18next';

const App = () => {
  const { t } = useTranslation();

return(
    <h1>{t('Title')}</h1>
);
```
以上で完了
