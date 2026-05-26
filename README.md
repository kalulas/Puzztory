# Puzztory

('/_ ')玩故事接龙的一个站点，承载了许多回忆的课程作业。

后端使用 django 开发，数据库选用 mysql。前端使用经典 jquery。

## Features

- 为故事接续片段
- 为接续故事限定关键词，限定接续长度
- 点赞：故事点赞 / 片段点赞 / 评论点赞
- 评论：故事评论 / 片段评论 / 回复评论

## Layout

```
.
│  database-design.md  # 数据库设计
│  manage.py
│  pyproject.toml
│  uv.lock
│  README.md
│
├─PuzzModel  # django 侧数据库声明
│
├─puzztory
│      settings.py     # 配置项
│      urls.py         # url 路由
│      user.py         # 用户相关
│      view.py         # 核心交互逻辑
│      ...
│
├─static     # 站点静态资源
│
└─templates  # 网页模板 html
```

## Prerequisites for deployment

- **Python** ≥ 3.10
- **uv** ≥ 0.11（[安装](https://docs.astral.sh/uv/getting-started/installation/)）
- **MySQL** 8.0+
- 生产环境建议：nginx 反代 + Let's Encrypt 证书 + systemd 进程托管

## Deployment

1. 手动在新设备上用 mysql 建库 puzztory

2. 拉代码、装依赖、应用 migration

   ```bash
   git clone https://github.com/kalulas/Puzztory.git
   cd Puzztory
   uv sync
   uv run python manage.py migrate
   ```

3. 本地开发用 Django 自带 runserver

   ```bash
   uv run python manage.py runserver [ip]:[port]
   ```

4. 生产推荐 nginx + gunicorn（unix socket）+ systemd

   ```bash
   # 单条命令试跑
   uv run gunicorn puzztory.wsgi:application \
       --workers 2 --threads 2 \
       --bind unix:/run/puzztory/gunicorn.sock
   ```

   nginx 反代 `/` 到 gunicorn unix socket，`/static` 直接 alias 项目的 static 目录即可。

5. You are good to go!

## License

[MIT](LICENSE)

