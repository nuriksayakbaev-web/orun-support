# ORUN Support — landing для link preview

Статическая страница (HTML + CSS, без JS, сборки, аналитики и форм). Нужна, чтобы ссылка на бота
https://t.me/OrunSupportBot показывалась в Telegram карточкой с заголовком, описанием и картинкой.

```
index.html
assets/orun-support-preview.png   # 1200×630, баннер из support-bot/assets/orun-welcome.png (обрезка по высоте + масштаб, логотип не менялся)
```

## 1. Подставьте публичный URL

В `index.html` везде замените `https://YOUR-USER.github.io/orun-support/` на реальный адрес страницы
(с `https://` и слэшем в конце). Затрагиваются `canonical`, `og:url`, `og:image`, `twitter:image`.
`og:image` должен быть абсолютным HTTPS-адресом картинки: `<ваш URL>assets/orun-support-preview.png`.

```
sed -i '' 's#https://YOUR-USER.github.io/orun-support/#https://ВАШ-АДРЕС/#g' index.html   # macOS
```

## 2. Локальная проверка

```
cd support-landing
python3 -m http.server 8080      # открыть http://localhost:8080
```

Проверьте адаптивность (узкое окно) и работу кнопки. Превью Telegram локально не покажется:
мета-теги читаются только по публичному HTTPS-адресу.

## 3. Публикация на GitHub Pages

1. Создайте репозиторий (например `orun-support`), положите в корень `index.html` и `assets/`.
2. Settings → Pages → Source: Deploy from a branch → `main` / `/ (root)`.
3. Через минуту страница будет на `https://<user>.github.io/orun-support/`. Убедитесь, что URL в тегах совпадает.
4. Картинка должна открываться напрямую по `og:image` (HTTP 200, `image/png`).

## 4. Проверка и сброс кеша Telegram

Отправьте ссылку себе в Telegram. Если карточка старая или пустая, напишите `@WebpageBot`, отправьте ему ссылку
и нажмите обновить — Telegram кеширует превью. После смены картинки меняйте имя файла или добавляйте `?v=2` к `og:image`.
