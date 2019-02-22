# Crates Api

This API provides access to crate data

## Get a crate

Get single crate by crate id.

```
GET api/crates/:crateId
```

### Response

```
Status: 200 OK
{
    "id": 92978,
    "name": "Weekly Essentials (May 11, 2018)",
    "url": "https://digitaldjpool.com/Crates/DigitalDJPoolCurator/92978/weekly-essentials-may-11-2018",
    "files_count": 22,
    "image_url": "//img.digitaldjpool.com//3/d/5/c/0/2/3d5c0218-0e06-4078-a02c-8da653e3d332.png",
    "files_url": "https://digitaldjpool.com/api/crates/92978/files",
    "created_date": "Wed, 09 May 2018 14:02:07 GMT",
    "updated_date": "Thu, 10 May 2018 14:27:17 GMT"
}
```

## List crate files

List all files contained in specified crate.

```
GET api/crates/:crateId/files
```
### Response

```
Status: 200 OK
[
    {
        "id": 169540,
        "artist": "Bad Royale ft Future Fambo",
        "title": "Hot Steppa",
        "version": "Dirty",
        "genre": "Dancehall",
        "url": "https://digitaldjpool.com/Songs/169540",
        "image_url": "//img.digitaldjpool.com//c/b/d/3/4/a/cbd34ae9-1b37-4c1b-a243-e42d838d0937.jpg",
        "preview_url": "https://digitaldjpool.com/api/files/Preview_Quality_Only_RbSdsacKmWElSmddScKRPC7tAXD8FZ9QzyR--sM_WYAERPm-wzyMEZVneoomqAwKtNyyl86cDjXJngGZMtXLfw2/preview",
        "download_url": "https://digitaldjpool.com/api/files/RbSdsacKmWElSmddScKRPC7tAXD8FZ9QzyR--sM_WYAERPm-wzyMEZVneoomqAwKtNyyl86cDjXJngGZMtXLfw2/download"
    },
    {
        "id": 169484,
        "artist": "Nicki Minaj",
        "title": "Chun Li",
        "version": "DJ Ominaya Party Break Edit Clean",
        "genre": "Hip-Hop",
        "url": "https://digitaldjpool.com/Songs/169484",
        "image_url": "//img.digitaldjpool.com//1/f/7/7/4/c/1f774c0c-cabf-4520-8e25-403237b6f982.jpg",
        "preview_url": "https://digitaldjpool.com/api/files/Preview_Quality_Only_Q2GtKXtq9j0ssAssU9hWG-XjHruS4lV2J1ZjaVvTiSwez-Om0MFu3NAlUjBUIJTMmx2a-qsz1iYH0OMBeOOcCg2/preview",
        "download_url": "https://digitaldjpool.com/api/files/Q2GtKXtq9j0ssAssU9hWG-XjHruS4lV2J1ZjaVvTiSwez-Om0MFu3NAlUjBUIJTMmx2a-qsz1iYH0OMBeOOcCg2/download"
    },
    {
        "id": 169405,
        "artist": "Marshmello & Anne Marie",
        "title": "Friends",
        "version": "Alphalove Remix",
        "genre": "Dance",
        "url": "https://digitaldjpool.com/Songs/169405",
        "image_url": "//img.digitaldjpool.com//2/7/e/e/9/3/27ee93bf-b6d0-461d-9cec-0a42de059f06.jpg",
        "preview_url": "https://digitaldjpool.com/api/files/Preview_Quality_Only_jQXc2mXGw95GItGLFUtSdYdNEWfm5Eqb24nE8wOz4f72_UP4QfMRSszQDt9Y-wo8hNdL0g7KWgm_Wd7KM134Ug2/preview",
        "download_url": "https://digitaldjpool.com/api/files/jQXc2mXGw95GItGLFUtSdYdNEWfm5Eqb24nE8wOz4f72_UP4QfMRSszQDt9Y-wo8hNdL0g7KWgm_Wd7KM134Ug2/download"
    }
]
```