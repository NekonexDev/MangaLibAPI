# MangaLibAPI
> Мини документация для MangaLib API
## Вступление
Приветствую, эта документация была создана ввиду отсутствия какой либо информации для разработчиков о внутренней части ресурса MangaLib. Приятного прочтения!
### Donate
Вы также можете отблагодарить меня по реквизитам ниже.

`TON: EQCPOg3vwwh79Hzk-9kaPMEvAERmXNCLQCGXhb921SMhlES-`

`BTC: bc1qvp97952xdkmph94zyhp6yulvwpjwyesz365nq9`

## Изпользование
*Запросы вы можете отправлять только по http2, иначе их заблокирует cloudflare.* 

Поддержка http2 имеется:
+ у библиотеки [httpx](https://www.python-httpx.org/http2/)(python)
+ у C#([клик](https://stackoverflow.com/questions/32685151/how-to-make-the-net-httpclient-use-http-2-0))
+ у JS([клик](https://www.npmjs.com/package/haxios))

Остальное думаю найдете сами.

Также хочу заметить, что некоторые endpoint-ы дублируются на сайтах и возвращают одно и тоже

## Документация

### Форум

`GET https://lib.social/api/faq/questions` - Вопросы

```
[
  {
    "title": "Что такое ранг?",
    "id": 4,
    "category_id": 2
  },
  {
    "title": "Что такое уровень?",
    "id": 5,
    "category_id": 2
  },
  {
    "title": "Что такое опыт?",
    "id": 6,
    "category_id": 2
  },
  ...
]
```
------
`GET https://lib.social/api/forum/discussion/id` - получение информации о треде

```
{
  "discussion": {
    "id": 123456,
    "chatter_category_id": 1,
    "title": "Привет bot",
    "user_id": 1397,
    "source_id": null,
    "source_type": null,
    "sticky": 2,
    "locked": 0,
    "views": 0,
    "answered": 0,
    "last_reply_at": "2023-03-07T21:28:29.000000Z",
    "created_at": "2023-02-18T16:29:43.000000Z",
    "updated_at": "2023-03-08T12:35:13.000000Z",
    "deleted_at": null,
    "yaoi": 0,
    "username": "serg1k",
    "avatar": "qq0gJSJ7KdWH.jpg",
    "category_id": 1,
    "category_name": "Баги и проблемы",
    "category_slug": "bugs",
    "category_color": "#f44336",
    "category_icon": "bug",
    "notification": false,
    "relation": null
  },
  "post": {
    "id": 1234567,
    "post_id": 0,
    "chatter_discussion_id": 123455,
    "user_id": 1397,
    "body": {
      "ops": []
    },
    "created_at": "2023-02-18T16:29:43.000000Z",
    "updated_at": "2023-03-04T08:03:28.000000Z",
    "deleted_at": null
  }
}
```
------
`GET https://lib.social/api/forum/posts?page=1&discussion_id=id` - посты в треде

*Примечание: да-да сайт публично показывает платформу с которой вы комментируете*

```
{
  "current_page": 1,
  "data": [
    {
      "id": 1234567,
      "post_id": 0,
      "chatter_discussion_id": 123456,
      "user_id": 1234567,
      "body": {
        "ops": [
          {
            "insert": "Привет bot!"
        ],
        "userAgent": {
          "platform": {
            "name": "AndroidOS",
            "version": "11"
          },
          "browser": {
            "name": "Chrome",
            "version": "110.0.0.0"
          },
          "device": "WebKit",
          "isDesktop": false,
          "isTablet": false,
          "isPhone": true
        }
      },
      "created_at": "2023-03-04T08:10:09.000000Z",
      "updated_at": "2023-03-04T08:10:09.000000Z",
      "deleted_at": null,
      "username": "serg1k",
      "avatar": "qq0gJSJ7KdWH.jpg",
      "reply_username": null,
      "reply_user_id": null
    }
  ]
}
```
------
`POST https://lib.social/api/forum/posts` - новый пост

**Требуется авторизация!**

```
{
  "chatter_discussion_id": id, // id треда куда публикуем
  "body": {
    ops: [
      {
        "insert": "Привет bot!"
      }
    ]
  }
}
```
------
`POST https://lib.social/api/forum/discussion` - новый тред

**Требуется авторизация!**

```
{
  "title": "Привет бот!",
  "body": {
    "ops": [
      {
        "insert": "Как дела?"
      }
    ]
  },
  "chatter_category_id": 7,
  "source_type": null,
  "source_id": null,
  "yaoi": false
}
```

### Пользователи
`GET https://mangalib.me/search?type=user&q=username` - поиск по нику

```
[
  {
    "id": 1234567,
    "value": "username",
    "avatar": "avatar_pic_id.jpg"
  }
]
```
------
`GET https://mangalib.me/uploads/users/{userid}/{avatar_pic_id}.jpg` - аватарка

`Возвращает аватарку`

### Контент сайта
`GET https://mangalib.me/search?type=manga&q=titlename` - поиск по названию

*Примечание: если вам требуется найти ранобэ/аниме просто замените домен, не меняя параметр type. Такой вот костыль.*

```
[
  {
    "id": 3757,
    "name": "Ijiranaide, Nagatoro-san",
    "rus_name": "Не издевайся, Нагаторо-сан",
    "eng_name": "Please don't bully me, Nagatoro",
    "slug": "ijiranaide-nagatoro-san",
    "status_id": 1,
    "otherNames": "イジらないで、長瀞さん / Iji-ranaide, Nagatoro-san / 괴롭히지 말아줘, 나가토로 양 / 不要欺負我、長瀞同學 / ยัยตัวแสบแอบน่ารัก นางาโทโระ / Uğraşma Benimle, Nagatoro / Nie drocz się ze mną, Nagatoro! /",
    "releaseDate": "2017",
    "summary": "Нагаторо - переведённая ученица в старшей школе, которой нравится дразнить своего семпая. Но он всё стерпит. И пусть ему придётся пройти через все виды издевательств - он всё простит, потому что влюблён.",
    "cover": "b4SpJNdF67Kj",
    "background": "zLQ39MddCwQE",
    "hot": 1,
    "caution": 1,
    "views": 5066550,
    "rate": 18688,
    "rate_avg": "4.81",
    "chap_count": 148,
    "type_id": 1,
    "user_id": 381497,
    "close_view": 0,
    "site": 1,
    "edit": 2,
    "moderated": 1,
    "created_at": "2018-01-20T21:07:27.000000Z",
    "updated_at": "2023-03-08T12:58:55.000000Z",
    "last_chapter_at": "2023-03-01 12:48:01",
    "moder_id": 1,
    "metadata": {
      "locked_fields": [
        "cover",
        "edit",
        "caution",
        "status_id",
        "manga_status_id",
        "publisher",
        "artists",
        "authors",
        "releaseDate",
        "type_id",
        "eng_name",
        "rus_name",
        "name"
      ],
      "announcement_link": "",
      "external_links": [
        "https://manganelo.com/manga/please_dont_bully_me_nagatoro",
        "https://pocket.shonenmagazine.com/episode/13932016480029136187",
        "https://www.mangaupdates.com/series.html?id=145530"
      ],
      "chapters": {
        "count": 1,
        "items": [
          {
            "id": 2354365,
            "number": "123",
            "volume": 16,
            "name": "Эй, Сэмпай...",
            "branch_id": null,
            "status": null
          }
        ]
      },
      "pricing_type": "1",
      "chapter_expired_type": "1"
    },
    "manga_status_id": 1,
    "views_day": 0,
    "views_week": 0,
    "views_month": 0,
    "rating_score": 4.497942177129953,
    "shiki_id": 110737,
    "value": "Ijiranaide, Nagatoro-san",
    "coverImage": "https://cover.imglib.info/uploads/cover/ijiranaide-nagatoro-san/cover/b4SpJNdF67Kj_250x350.jpg",
    "coverImageThumbnail": "https://cover.imglib.info/uploads/cover/ijiranaide-nagatoro-san/cover/b4SpJNdF67Kj_thumb.jpg",
    "href": "https://mangalib.me/ijiranaide-nagatoro-san",
    "covers": {
      "default": "https://cover.imglib.info/uploads/cover/ijiranaide-nagatoro-san/cover/b4SpJNdF67Kj_250x350.jpg",
      "thumbnail": "https://cover.imglib.info/uploads/cover/ijiranaide-nagatoro-san/cover/b4SpJNdF67Kj_thumb.jpg"
    },
    "title_status_id": 1
  }
]
```
