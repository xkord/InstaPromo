# Раскрутка Instagram с помощью InstaPy

Есть готовый docker контейнер https://github.com/InstaPy/instapy-docker

```sh
git clone https://github.com/InstaPy/instapy-docker.git
cd docker-compose
cp -a docker_quickstart.py.example docker_quickstart.py
docker-compose pull && docker-compose up -d --build web
# Stop InstaPy container 
# docker-compose stop web
# Stop and remove Docker configs 
# docker-compose down
docker logs -f instapy_web_1 
# or
# docker logs --tail 50 -f $(docker ps -a | grep instapy_web | cut -d " " -f 1)
# Edit your crontab file
# 30 8 * * * cd /path_to_repo/docker-compose/ && /usr/local/bin/docker-compose up -d --build > /root/log 2>&1
```

## Пример docker_quickstart.py

```python
from instapy import *

insta_username = '***'
insta_password = '***'

dont_like = ['food', 'girl', 'hot']
ignore_words = ['pizza']
friend_list = ['friend1', 'friend2', 'friend3']

bot = InstaPy(username=insta_username, password=insta_password, headless_browser=True)
with smart_run(bot):
    bot.set_relationship_bounds(enabled=True,
                                potency_ratio=-1.21,
                                delimit_by_numbers=True,
                                max_followers=4590,
                                max_following=5555,
                                min_followers=45,
                                min_following=77)
    bot.set_do_follow(enabled=True, percentage=100)
    bot.set_do_comment(False, percentage=10)
    bot.set_comments(['Cool!', 'Awesome!', 'Nice!'])
    bot.set_dont_include(friend_list)
    bot.set_dont_like(dont_like)
    bot.set_ignore_if_contains(ignore_words)
    bot.unfollow_users(amount=1000, InstapyFollowed=(True, "nonfollowers"), style="FIFO", unfollow_after=12*60*60, sleep_delay=601)
    bot.like_by_tags(['нервы', '#нервы'], amount=1000)
    bot.end()
```