---

title: 配置hexo-algoliasearch
date: 2024-10-04 00:00:00
categories: 教程
tags:

- hexo

cover: https://i.loli.net/2020/12/15/CBjwueKp3oFsyZX.jpg
---

[](https://github.com/LouisBarranqueiro/hexo-algoliasearch#hexo-algoliasearch)

[![npm version](https://camo.githubusercontent.com/9b0ab22707c489784060e036675bc88f10822d30edef729c3f64fe81c2c348fd/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f6865786f2d616c676f6c69617365617263683f7374796c653d666c61742d737175617265)](https://www.npmjs.com/package/hexo-algoliasearch) [![npm download/month](https://camo.githubusercontent.com/900816402f44f2808a4a34aa58ea1d17993f1381dc4a16db2f057bfded1c2660/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f646d2f6865786f2d616c676f6c69617365617263682e7376673f7374796c653d666c61742d737175617265)](https://www.npmjs.com/package/hexo-algoliasearch) [![code coverage](https://camo.githubusercontent.com/0c6302f0962662ee20c080d1f026c15c6389dbb42d9ae7e766a3143f1c0c493c/68747470733a2f2f696d672e736869656c64732e696f2f636f6465636f762f632f6769746875622f4c6f75697342617272616e71756569726f2f6865786f2d616c676f6c69617365617263683f7374796c653d666c61742d737175617265)](https://app.codecov.io/gh/LouisBarranqueiro/hexo-algoliasearch/tree/main)

A plugin to index posts of your Hexo blog on Algolia

## Installation

[](https://github.com/LouisBarranqueiro/hexo-algoliasearch#installation)

```
npm install hexo-algoliasearch --save
```

If Hexo detect automatically all plugins, that's all.

If that is not the case, register the plugin in your `_config.yml` file :

```yaml
plugins:  - hexo-algoliasearch
```

## Configuration

[](https://github.com/LouisBarranqueiro/hexo-algoliasearch#configuration)

You can configure this plugin in your `_config.yml` file :

```yaml
algolia:  appId: "Z7A3XW4R2I"
  apiKey: "12db1ad54372045549ef465881c17e743"
  adminApiKey: "40321c7c207e7f73b63a19aa24c4761b"
  chunkSize: 5000
  indexName: "my-hexo-blog"
  fields:    - content:strip:truncate,0,500
    - excerpt:strip
    - gallery
    - permalink
    - photos
    - slug
    - tags
    - title
```

| Key         | Type   | Default | Description                                                                                                                                                                                                                                               |
| ----------- | ------ | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| appId       | String |         | Your application ID. Optional, if the environment variable `ALGOLIA_APP_ID` is set                                                                                                                                                                        |
| apiKey      | String |         | Your API key (read only). It is use to search in an index. Optional, if the environment variable `ALGOLIA_API_KEY` is set                                                                                                                                 |
| adminApiKey | String |         | Your adminAPI key. It is use to create, delete, update your indexes. Optional, if the environment variable `ALGOLIA_ADMIN_API_KEY` is set                                                                                                                 |
| chunkSize   | Number | 5000    | Records/posts are split in chunks to upload them. Algolia recommend to use `5000` for best performance. Be careful, if you are indexing post content, It <br>can fail because of size limit. To overcome this, decrease size of <br>chunks until it pass. |
| indexName   | String |         | The name of the index in which posts are stored. Optional, if the environment variable `ALGOLIA_INDEX_NAME` is set                                                                                                                                        |
| fields      | List   |         | The list of the field names to index. Separate field name and filters with `:`. Read [Filters](https://github.com/LouisBarranqueiro/hexo-algoliasearch#filters) for more information                                                                      |

#### Filters

[](https://github.com/LouisBarranqueiro/hexo-algoliasearch#filters)

Filters give you the ability to process value of fields before indexation.
Filters are separated each others by colons (`:`) and may have optional arguments separated by commas (`,`).
Multiple filters can be chained. The output of one filter is applied to the next.

##### List of filters:

[](https://github.com/LouisBarranqueiro/hexo-algoliasearch#list-of-filters)

| Filter   | Signature                              | Syntax           | Description                                                                                                                                   |
| -------- | -------------------------------------- | ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| strip    | `strip()`                              | `strip`          | Strip HTML. It can be useful for excerpt and content value to not index HTML tags and attributes.                                             |
| truncate | `truncate(start: number, end: number)` | `truncate,0,300` | Truncate string from `start` index to `end` index. Algolia has some limitations about record size so it might be useful to cut post contents. |

##### Example

[](https://github.com/LouisBarranqueiro/hexo-algoliasearch#example)

  fields:
    - content:strip:truncate,0,200

It will strip HTML from `content` value then truncate the result starting from index `0` to index `200` before indexation.
This property will be added to algolia records as `contentStripTruncate`

## Usage

[](https://github.com/LouisBarranqueiro/hexo-algoliasearch#usage)

```
hexo algolia
```

#### Options

[](https://github.com/LouisBarranqueiro/hexo-algoliasearch#options)

| Options        | Description                       |
| -------------- | --------------------------------- |
| -n, --no-clear | Does not clear the existing index |

# Licence

[](https://github.com/LouisBarranqueiro/hexo-algoliasearch#licence)

hexo-algoliasearch is under [MIT](https://github.com/LouisBarranqueiro/hexo-algoliasearch/blob/master/LICENSE)
