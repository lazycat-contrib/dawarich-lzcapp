# Dawarich 懒猫微服包

自托管的位置历史记录应用。把设备上报的定位数据收集起来，在地图、时间线、统计和行程里回看。

- 上游：<https://github.com/Freika/dawarich>
- 版本：跟随上游镜像 `freikin/dawarich` 的三段式标签
- 许可：AGPL-3.0（全文见 `content/licenses/LICENSE`）

## 组成

| 服务 | 镜像 | 作用 |
| --- | --- | --- |
| `dawarich` | `freikin/dawarich` | Rails 应用，监听 3000 |
| `db` | `postgis/postgis:17-3.5-alpine` | PostgreSQL + PostGIS，存地理轨迹 |
| `redis` | `redis:8-alpine` | 后台任务队列与缓存 |

## 关键配置说明

- **PostGIS 是必需的**，普通 PostgreSQL 缺少地理类型，应用无法建表。
- `SECRET_KEY_BASE` 必须覆盖：上游默认值是字面量 `CHANGE_ME`，
  留着会让会话与加密属性可被伪造。
- `APPLICATION_HOSTS` 必须含应用域名：上游默认只允许 localhost，
  反代过来的 Host 头会被拒绝。
- `ALLOW_EMAIL_PASSWORD_REGISTRATION` 必须显式设为 `true`：上游在自托管模式下
  默认关闭邮箱注册（只留家庭邀请），不打开就没有可用的首次建号路径。
- 应用自带账号体系，因此 `public_path` 开放根路径，由应用自己鉴权。

## 未验证部分

未在真机上实测：注册登录、位置上报入库、地图与时间线展示、重启后数据留存。
