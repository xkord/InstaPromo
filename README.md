# Раскрутка Instagram с помощью InstaPy

Есть готовый docker контейнер https://github.com/InstaPy/instapy-docker

```sh
docker pull instapy/instapy
mkdir /root/instapy_data
cd /root/instapy_data

touch run.sh
chmod +x run.sh

# run.sh -----------------------------------------------
#!/bin/bash
docker run --name $1\
  -v /root/instapy_data/$1/docker_quickstart.py:/code/docker_quickstart.py\
  -v /root/instapy_data/$1/InstaPy:/code/InstaPy\
  -d instapy/instapy
docker container start $1
# ------------------------------------------------------

mkdir osobyegosti
cd osobyegosti
# create docker_quickstart.py - see example below

crontab -e
# 08 12 * * * /root/instapy_data/run.sh osobyegosti
```

## Пример docker_quickstart.py

```python
from instapy import *
import random
# Write your automation here
# Stuck ? Look at the github page or the examples in the examples folder

insta_username = 'osobyegosti'
insta_password = '**********'

dont_like = ['food', 'girl', 'hot']
ignore_words = ['pizza']
friend_list = ['friend1', 'friend2', 'friend3']
# TARGET data
""" Set similar accounts and influencers from your niche to target...
"""
targets = ['nervy_official', 'yuriykaplan', 'b2band']
# If you want to enter your Instagram Credentials directly just enter
# username=<your-username-here> and password=<your-password> into InstaPy
# e.g like so InstaPy(username="instagram", password="test1234")

bot = InstaPy(username=insta_username, password=insta_password, headless_browser=True)
with smart_run(bot):
    # FOLLOW+INTERACTION on TARGETED accounts
    """ Select users form a list of a predefined targets...
    """
    number = random.randint(3, 5)
    random_targets = targets

    if len(targets) <= number:
        random_targets = targets

    else:
        random_targets = random.sample(targets, number)

    #bot.set_relationship_bounds(enabled=True,
    #                            potency_ratio=-1.21,
    #                            delimit_by_numbers=True,
    #                            max_followers=4590,
    #                            max_following=5555,
    #                            min_followers=45,
    #                            min_following=77)
    bot.set_do_follow(enabled=True, percentage=100)
    bot.set_do_comment(False, percentage=10)
    # bot.set_comments(['Cool!', 'Awesome!', 'Nice!'])
    bot.set_dont_include(friend_list)
    bot.set_dont_like(dont_like)
    bot.set_ignore_if_contains(ignore_words)
    bot.unfollow_users(amount=1000, InstapyFollowed=(True, "nonfollowers"), style="FIFO", unfollow_after=12*60*60, sleep_delay=601)
    bot.follow_user_followers(random_targets, amount=random.randint(30, 60), randomize=True, sleep_delay=600, interact=True)
    bot.like_by_tags(['группанервы', '#группанервы', 'русскийрок', 'рок'], amount=1000)
    bot.end()
```

## Создание swap файла при запуске нескольких пользователей на VPS

```sh
cd /var
touch swap.img
chmod 600 swap.img
dd if=/dev/zero of=/var/swap.img bs=2048k count=1000
mkswap /var/swap.img
swapon /var/swap.img
# swapoff /var/swap.img 
echo "/var/swap.img    none    swap    sw    0    0" >> /etc/fstab
```