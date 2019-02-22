# Users API

## List user crates

List all crates created by user.

```
GET api/users/:userName/crates
Response
Status: 200 OK
[
    {
        "id": 63226,
        "name": "Default",
        "url": "https://digitaldjpool.com/Crates/virab/63226/default",
        "files_count": 1,
        "image_url": null,
        "files_url": "https://digitaldjpool.com/api/crates/63226/files",
        "created_date": "Mon, 14 Dec 2015 15:02:55 GMT",
        "updated_date": "Mon, 14 Dec 2015 15:02:55 GMT"
    },
    {
        "id": 63227,
        "name": "First Crate",
        "url": "https://digitaldjpool.com/Crates/virab/63227/first-crate",
        "files_count": 2,
        "image_url": null,
        "files_url": "https://digitaldjpool.com/api/crates/63227/files",
        "created_date": "Mon, 14 Dec 2015 15:02:55 GMT",
        "updated_date": "Mon, 14 Dec 2015 15:02:55 GMT"
    }
]
```