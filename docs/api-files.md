# Files API

## Get file preview url

Get tokenized file preview url

```
GET api/files/:id/preview
```

### Response

```
Status: 200 OK
{
    "fileId": 183605,
    "http_mp3_preview": "https://digitaldjpool.com/Media/PreviewMp3/183605?token=sCEJC26N91YBd3QnelyPu6B3-nW4tAjKkqdY05z0rqL_oBoVX8AbMpstae1PKc2IdHpbvLsSJhLwOffCe86sSvfZKir6CWKdYZVQsJzRP9cZOJ4TbECvWIiHzDNFY5vkZUvjngPix1g74YMLUrWtKjr49x050jS0wy4WnFiet3NyHnjT8qS8VF8ATeTM06ncWhskiuojJbJyQ-Jx-oHJOxtL36Rl4-k-tzKVZUi-xUEXHb1wTBSgchqn_rvLg7gn0"
}
```

## Get file download url

Get tokenized file download url

```
GET api/files/:id/download
```

### Response

```
Status: 200 OK
{
    "fileId":183605,
    "http_mp3_download":"https://digitaldjpool.com/media/downloadsas/183605?token=sCEJC26N91YBd3QnelyPuzlxudEysZdlkqEbbr1cghJkEaUijoPpVIfinzYWB9jgzsgXjRnjx2gnDKHEgfwrXBmc5B_fP7N6B9zdsLMOlJ_bQsU8gJx-hId7X2jzjpVxLh5ukjt1YQIFG3hbfxXzVvnNBprHXjipgxPmY5wxpc-gLVqznM6LojQGBSuj956yfdZyVBrrj-v08jYLEsrJNsUY6BZdBmMup7pAZUi3FTrHBM50stzvggAuJFtMxwMQ0"
}
```