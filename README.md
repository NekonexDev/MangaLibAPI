# MangaLibAPI
> Мини документация для MangaLib API
## Вступление
Приветствую, эта документация была создана ввиду отсутствия какой либо информации для разработчиков о внутренней части ресурса MangaLib. Приятного прочтения!
### Donate
Вы также можете отблагодарить меня по реквизитам ниже.

`TON: EQCPOg3vwwh79Hzk-9kaPMEvAERmXNCLQCGXhb921SMhlES-`

`BTC: bc1qvp97952xdkmph94zyhp6yulvwpjwyesz365nq9`

## Использование
*Запросы вы можете отправлять только по http2, иначе их заблокирует cloudflare.* 

Поддержка http2 имеется:
+ у библиотеки [httpx](https://www.python-httpx.org/http2/)(python)
+ у C#([клик](https://stackoverflow.com/questions/32685151/how-to-make-the-net-httpclient-use-http-2-0))
+ у JS([клик](https://www.npmjs.com/package/haxios))

Остальное думаю найдете сами.

Также хочу заметить, что некоторые endpoint-ы дублируются на сайтах и возвращают одно и тоже.

При желании дополнить, можете кинуть pull-request.

## Документация

```
// Список констант
site_id
 + animelib - 5
 + mangalib - 1
 + ranobelib - 3
 + yaoilib - 2
```
### Авторизация
`POST https://lib.social/login` - войти

```
{
  "_token": "", // берем с главной QuerySelector("meta[name=_token]")
  "login": "", // почта
  "password": "", // пароль
  "remember": "on"
}
```

*В итоге: Нам кидают ответ, берете location хедер и все, задача онли почистить и токен у вас*

### Форум

`POST https://lib.social/api/forum/discussion/id/action` - действия с тредом

**Требуется авторизация!**

```
// Закрыть тред
{
    actionType: locked
}
```

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
------
`GET https://mangalib.me/manga-short-info?id=55901&slug=chainsaw-man-2` - получение информации по id/slug

*Примечание: вы можете добавить параметр type, если знаете тип материала.*

Type может быть:
+ anime
+ manga

```
{
  "first_chapter": {
    "volume": 12,
    "number": "98"
  },
  "id": 55901,
  "name": "Chainsaw Man 2",
  "rus_name": "Человек-бензопила 2",
  "eng_name": "Chainsaw Man 2",
  "slug": "chainsaw-man-2",
  "releaseDate": "2022",
  "summary": "Прямое продолжение первой части.",
  "cover": "Gm9UeJU3bTpz",
  "caution": 2,
  "rate": 18948,
  "rate_avg": "4.89",
  "authors": [
    {
      "name": "Tatsuki Fujimoto",
      "slug": "tatsuki-fujimoto",
      "id": 93024,
      "pivot": {
        "manga_id": 55901,
        "author_id": 93024
      }
    }
  ],
  "artists": [
    {
      "name": "Tatsuki Fujimoto",
      "slug": "tatsuki-fujimoto",
      "id": 93024,
      "pivot": {
        "manga_id": 55901,
        "author_id": 93024
      }
    }
  ],
  "categories": [
    {
      "id": 34,
      "name": "боевик",
      "slug": "action",
      "dsc": "Работа, содержащая насилие, драки, схватки и быстрыми сменами действия.",
      "created_at": "2016-03-05T11:28:35.000000Z",
      "updated_at": "2016-03-30T22:36:49.000000Z",
      "pivot": {
        "media_id": 55901,
        "genre_id": 34,
        "media_type": "manga"
      }
    },
    ...
  ],
  "tags": [
    {
      "id": 151,
      "name": "Демоны",
      "slug": "150-demony",
      "site_id": 1,
      "created_at": "2020-01-06T07:36:12.000000Z",
      "updated_at": "2020-01-06T07:36:12.000000Z",
      "pivot": {
        "media_id": 55901,
        "tag_id": 151,
        "media_type": "manga"
      }
    },
    ...
  ],
  "status": {
    "id": 1,
    "label": "Продолжается",
    "created_at": "2016-03-05 16:35:36",
    "updated_at": "0000-00-00 00:00:00",
    "type": 1
  },
  "coverImage": "https://cover.imglib.info/uploads/cover/chainsaw-man-2/cover/Gm9UeJU3bTpz_250x350.jpg",
  "href": "https://mangalib.me/chainsaw-man-2"
}
```
------
`POST https://mangalib.me/api/list` - список тайтлов

*Примечание: на сайте(ах) очень много фильтров, этот endpoint изпользуется почти везде, где требуется запросить несколько тайтлов(тайтлы переводчиков, каталог, тайтлы авторов и т.д.), поэтому будут указаны лишь базовые фильтры. <Как найти остальные?> - Очень просто! - Открываете нужную вам страничку и DevTools на ней, выбираете Network, ставите пункт Fetch/XHR и меняете фильтр на сайте(см. картинку) на нужный вам, смотрите в запросы и находите запрос list, тыкаете на него, вкладка Payload.*

![картинка1](https://user-images.githubusercontent.com/97462968/223760616-e9aba766-7f13-42fe-b748-7595851c2e87.png)

```
// Фильтры с главной RanobeLib
{
  "sort": "rate",
  "dir": "desc",
  "page": 1,
  "types": [
    "10"
  ],
  "site_id": "3",
  "type": "manga",
  "caution_list": [
    "Отсутствует",
    "16+",
    "18+"
  ]
}

```
------
`GET https://mangalib.me/similar/id` - похожие по id

```
{
  "items": [
    {
      "id": 7276,
      "slug": "zettai-junshukyousei-kozukuri-kyokashou-anime",
      "name": "Zettai Junshu☆Kyousei Kozukuri Kyokashou!!",
      "rus_name": "Абсолютная покорность: Секс по лицензии",
      "eng_name": null,
      "cover": "1568f96a-45cf-4eae-9f6f-1fd33f24be3a",
      "type_id": 20,
      "status_id": 7,
      "anime_status_id": 2,
      "type_text": "",
      "status_text": "",
      "userVote": null,
      "subtitle": "Схож по жанрам",
      "title": {
        "id": 7276,
        "slug": "zettai-junshukyousei-kozukuri-kyokashou-anime",
        "name": "Zettai Junshu☆Kyousei Kozukuri Kyokashou!!",
        "rus_name": "Абсолютная покорность: Секс по лицензии",
        "eng_name": null,
        "cover": "1568f96a-45cf-4eae-9f6f-1fd33f24be3a",
        "type_id": 20,
        "status_id": 7,
        "anime_status_id": 2,
        "type_text": "",
        "status_text": "",
        "userVote": null,
        "subtitle": "Схож по жанрам",
        "covers": {
          "default": "https://animelib.me/uploads/anime/7276/cover/1568f96a-45cf-4eae-9f6f-1fd33f24be3a.jpg",
          "thumbnail": "/uploads/anime/7276/cover/1568f96a-45cf-4eae-9f6f-1fd33f24be3a_thumb.jpg"
        },
        "coverImage": "https://animelib.me/uploads/anime/7276/cover/1568f96a-45cf-4eae-9f6f-1fd33f24be3a.jpg",
        "coverImageThumbnail": "/uploads/anime/7276/cover/1568f96a-45cf-4eae-9f6f-1fd33f24be3a_thumb.jpg",
        "href": "https://mangalib.me/anime/7276-zettai-junshukyousei-kozukuri-kyokashou-anime",
        "title_status_id": 2
      },
      "covers": {
        "default": "https://animelib.me/uploads/anime/7276/cover/1568f96a-45cf-4eae-9f6f-1fd33f24be3a.jpg",
        "thumbnail": "/uploads/anime/7276/cover/1568f96a-45cf-4eae-9f6f-1fd33f24be3a_thumb.jpg"
      },
      "coverImage": "https://animelib.me/uploads/anime/7276/cover/1568f96a-45cf-4eae-9f6f-1fd33f24be3a.jpg",
      "coverImageThumbnail": "/uploads/anime/7276/cover/1568f96a-45cf-4eae-9f6f-1fd33f24be3a_thumb.jpg",
      "href": "https://mangalib.me/anime/7276-zettai-junshukyousei-kozukuri-kyokashou-anime",
      "title_status_id": 2
    },
    ...
  ]
}
```
