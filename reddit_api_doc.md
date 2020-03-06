# Documenting the Reddit API

This will walk through the Reddit API.

Reddit is a social network where an original poster (OP) starts a conversation by beginning a thread. Others respond, and a heirarchy of responses form underneath. People can reward other answers thereby shifting them up by upvoting certain posts or responses. Different topics of conversation are siloed into what are called "subreddits," and there are subreddits for any topic that you can think of like asian beauty products, to politics, to caring for a plant.

The Reddit API will let you do things like check various updates with your own account, post, read comments, and pull batches of posts, or search for posts, and much more. The API methods can be grouped by their autorization scope. If, for example, you are a mod of a subreddit, which means that you have special powers to delete others posts in the subreddit and make other modifications to the rules that that community must abide by, you will get special permissions in the API to be able to replicate that behavior.

First go deal with the authorization that you would need to interact with the API.

1. First, create and account at reddit.com.
2. After that, go to https://www.reddit.com/prefs/apps and click create app.
3. Fill in the fields for indicating what you want to do with the app, including the purpose, a small decription, a URI (which can be local host for testing) and then click create.
4. You will get back a Client ID, and a Client Secret. Export those variables in the terminal so you will have access to them in your python script.

Next, you will need an autorization token which is only valid for a limited period of time. You can get this by writing the following script
The User Agent part isn't important and can be anything. Also, keep in mind that the bearer token is only good for 60 minutes, and you will have to do this again
in an hour.


```python
import requests
import requests.auth as auth
import json
```


```python
client_auth = requests.auth.HTTPBasicAuth('5kx57BjRfsdtyg', 'ZXxBCkSaG5Gf9vDiLKMbUP7cMbs')
post_data = {"grant_type": "password", "username": "bakebreadsmellroses", "password": "banana-hammock"}
headers = {"User-Agent": "ChangeMeClient/0.1 by YourUsername"}

# Make a POST request with information about authorization to get back a json object in response
# which contains the auth token
response = requests.post("https://www.reddit.com/api/v1/access_token", auth=client_auth, data=post_data, headers=headers)
response.json()
```




    {'access_token': '272808207682-Ei_jijNHhU9YIyHbnoCkw-AuwgA',
     'token_type': 'bearer',
     'expires_in': 3600,
     'scope': '*'}


Requests are made to the /api/, /message/, /about/, /wiki/, and /subreddit/ end points.

For example, here is an end point that would tell you about yourself:


```python
"https://oauth.reddit.com/api/v1/me"
```




    'https://oauth.reddit.com/api/v1/me'



Here's an endpoint that would submit to a subreddit:


```python
"https://oauth.reddit.com/api/submit"
```




    'https://oauth.reddit.com/api/submit'



Or to view subreddits that are currently new, and we'll use it for the following examples as well:


```python
"https://oauth.reddit.com/subreddits/new"
```




    'https://oauth.reddit.com/subreddits/new'




Now that we have the auth token, we can make requests to oauth.reddit.com/ and the endpoint that we want to hit. For example, let's find out what new subreddits there are!


```python
# Headers that contain the auth token
headers = {"Authorization": "INSERT HERE", "User-Agent": "ChangeMeClient/0.1 by YourUsername"}

# Make a GET requestion with the headers, the endpoint, and any parameters, which returns
# a response object in JSON, which can be parsed for desired output
response = requests.get("https://oauth.reddit.com/subreddits/new", headers=headers)
for item in response.json()['data']['children']:
    print(item['data']['title'])

```

    GoAndGarrett
    Boogbois
    Anythingfunne
    EchoScreen
    AAAAAAAAAAAAH
    Earn CCP social credits for good behavior
    Test
    WateredMilk
    QuibiApp
    BookerAlert
    TheNuclearDom
    PC2MasterRace
    Hemşirelik Gündemi
    AayalaValentine
    Jordyn Chang
    SAOSideStoryfanmade
    blackboxengineering
    NetflixNextMovie
    SchoolarHell
    ShrimpMemez
    Swae
    PuddlePhotography
    UFC 247 Jones vs Reyes Live Stream on Reddit links Now
    HTLYE


Cool, so some new subreddits include one for people like photograph of puddles, Netflix movies, and black box engineering etc...

You can also give parameters to the endpoints by doing the following:


```python
# Here I gave some parameters that would limit the number of results I got to 1
params = dict(limit=1)
response = requests.get("https://oauth.reddit.com/subreddits/new", headers=headers, params=params)
response.json()
```




    {'kind': 'Listing',
     'data': {'modhash': None,
      'dist': 1,
      'children': [{'kind': 't5',
        'data': {'user_flair_background_color': None,
         'submit_text_html': None,
         'restrict_posting': True,
         'user_is_banned': False,
         'free_form_reports': True,
         'wiki_enabled': None,
         'user_is_muted': False,
         'user_can_flair_in_sr': None,
         'display_name': 'Dozlin',
         'header_img': None,
         'title': 'Dozlin',
         'icon_size': None,
         'primary_color': '',
         'active_user_count': None,
         'icon_img': '',
         'display_name_prefixed': 'r/Dozlin',
         'accounts_active': None,
         'public_traffic': False,
         'subscribers': 1,
         'user_flair_richtext': [],
         'videostream_links_count': 0,
         'name': 't5_2eszp3',
         'quarantine': False,
         'hide_ads': False,
         'emojis_enabled': False,
         'advertiser_category': '',
         'public_description': 'A community that exists to observe, speculate upon, and study the popular Minecraft youtuber Dozlin',
         'comment_score_hide_mins': 0,
         'user_has_favorited': False,
         'user_flair_template_id': None,
         'community_icon': '',
         'banner_background_image': '',
         'original_content_tag_enabled': False,
         'submit_text': '',
         'description_html': '&lt;!-- SC_OFF --&gt;&lt;div class="md"&gt;&lt;p&gt;A community that exists to observe, speculate upon, and study the popular Minecraft youtuber Dozlin&lt;/p&gt;\n&lt;/div&gt;&lt;!-- SC_ON --&gt;',
         'spoilers_enabled': True,
         'header_title': '',
         'header_size': None,
         'user_flair_position': 'right',
         'all_original_content': False,
         'has_menu_widget': False,
         'is_enrolled_in_new_modmail': None,
         'key_color': '',
         'can_assign_user_flair': False,
         'created': 1581120840.0,
         'wls': None,
         'show_media_preview': True,
         'submission_type': 'any',
         'user_is_subscriber': False,
         'disable_contributor_requests': False,
         'allow_videogifs': True,
         'user_flair_type': 'text',
         'allow_polls': False,
         'collapse_deleted_comments': False,
         'emojis_custom_size': None,
         'public_description_html': '&lt;!-- SC_OFF --&gt;&lt;div class="md"&gt;&lt;p&gt;A community that exists to observe, speculate upon, and study the popular Minecraft youtuber Dozlin&lt;/p&gt;\n&lt;/div&gt;&lt;!-- SC_ON --&gt;',
         'allow_videos': True,
         'is_crosspostable_subreddit': True,
         'suggested_comment_sort': None,
         'can_assign_link_flair': False,
         'accounts_active_is_fuzzed': False,
         'submit_text_label': '',
         'link_flair_position': '',
         'user_sr_flair_enabled': None,
         'user_flair_enabled_in_sr': False,
         'allow_chat_post_creation': True,
         'allow_discovery': True,
         'user_sr_theme_enabled': True,
         'link_flair_enabled': False,
         'subreddit_type': 'public',
         'notification_level': None,
         'banner_img': '',
         'user_flair_text': None,
         'banner_background_color': '',
         'show_media': True,
         'id': '2eszp3',
         'user_is_contributor': False,
         'over18': False,
         'description': 'A community that exists to observe, speculate upon, and study the popular Minecraft youtuber Dozlin',
         'is_chat_post_feature_enabled': True,
         'submit_link_label': '',
         'user_flair_text_color': None,
         'restrict_commenting': False,
         'user_flair_css_class': None,
         'allow_images': True,
         'lang': 'en',
         'whitelist_status': None,
         'url': '/r/Dozlin/',
         'created_utc': 1581092040.0,
         'banner_size': None,
         'mobile_banner_image': '',
         'user_is_moderator': False}}],
      'after': 't5_2eszp3',
      'before': None}}



What I got back here is one example of a subreddit. This is some subreddit named
Dozlin and and it doesn't have a background color on its page, posting is
restricted, I am not banned from it, and there is only one subscriber, is some
of the information that you can get from the json that is returned.
Here are some of the parameters that you could have passed in to this endpoint:

1. after=[a "fullname"] After will give you enteries that come after the "fullname" of the id of a post. Fullnames consist of somethings "type" and its unique id which then forms a globally unique id on reddit. They start with the prefix for the objects type and then the things unique id in base 36. so for example, the full name for the example above would be t5_2esw3n.

  * t1's are comments,
  * t2's are accounts,
  * t3's are links,
  * t4's are messages,
  * t5's are subreddits and
  * t6's are awards
2. before=[a "fullname"] Before will give you enteries that come before the "fullname" of the id of a post.
3. limit=[a number] which is the maximum number of enteries that you would like to be returned

We get back information like the above that includes all sorts of information about the subreddit like its title, id, if it allows images, its url, the descriptions, if its over 18 and so on.

Let's try a few more queries to look at other information. We can run a search for subreddits we are looking for or match key terms.


```python
params = dict(limit=1, q="pineapple clues")
response = requests.get("https://oauth.reddit.com/subreddits/search", headers=headers, params=params)
response.json()
```




    {'kind': 'Listing',
     'data': {'modhash': None,
      'dist': 1,
      'children': [{'kind': 't5',
        'data': {'user_flair_background_color': None,
         'submit_text_html': None,
         'restrict_posting': True,
         'user_is_banned': False,
         'free_form_reports': True,
         'wiki_enabled': None,
         'user_is_muted': False,
         'user_can_flair_in_sr': None,
         'display_name': 'psych',
         'header_img': 'https://d.thumbs.redditmedia.com/FN5wQoKXWKQ_i4qQ.png',
         'title': "Psych - You know that's right.",
         'icon_size': [256, 256],
         'primary_color': '#94e044',
         'active_user_count': None,
         'icon_img': 'https://a.thumbs.redditmedia.com/s9LqY0vCxzmZUVjKynes8zNEelxHusbYujMZDkcMLG0.png',
         'display_name_prefixed': 'r/psych',
         'accounts_active': None,
         'public_traffic': False,
         'subscribers': 45380,
         'user_flair_richtext': [],
         'videostream_links_count': 0,
         'name': 't5_2qxd2',
         'quarantine': False,
         'hide_ads': False,
         'emojis_enabled': False,
         'advertiser_category': '',
         'public_description': "A subreddit devoted to all things Psych. If you're a fan of the silliest TV show on USA Network, slice open a pineapple and subscribe today!\n\n(This is not a community about psychology or psychiatry! Try /r/psychology or a related subreddit for that, or /r/SampleSize if looking to take or share surveys.)",
         'comment_score_hide_mins': 0,
         'user_has_favorited': False,
         'user_flair_template_id': None,
         'community_icon': 'https://styles.redditmedia.com/t5_2qxd2/styles/communityIcon_vggtd3yd8lb31.jpg',
         'banner_background_image': '',
         'original_content_tag_enabled': False,
         'submit_text': '',
         'description_html': '&lt;!-- SC_OFF --&gt;&lt;div class="md"&gt;&lt;h3&gt;&lt;em&gt;Psych: The Movie&lt;/em&gt; &lt;a href="https://www.reddit.com/r/psych/comments/7iao3w/s09e01_psych_the_movie_preshow_discussion_thread/"&gt;Pre-Show Discussion&lt;/a&gt; | &lt;a href="https://www.reddit.com/r/psych/comments/7ib47u/s09e01_psych_the_movie_live_discussion_thread/"&gt;Live Discussion&lt;/a&gt; | &lt;a href="https://www.reddit.com/r/psych/comments/7ibvb1/s09e01_psych_the_movie_postshow_discussion_thread/"&gt;Post-Show Discussion&lt;/a&gt;&lt;/h3&gt;\n\n&lt;p&gt;&lt;strong&gt;A subreddit devoted to all things &lt;em&gt;Psych&lt;/em&gt;!&lt;/strong&gt; This is the place to talk about the silliest, most pineapple-filled show on USA Network.&lt;/p&gt;\n\n&lt;p&gt;Show Summary:&lt;/p&gt;\n\n&lt;blockquote&gt;\n&lt;p&gt;Shawn Spencer has developed a keen eye for detail after being instructed by his police officer father to note even the most minute details of his surroundings. After conning the police into believing that he&amp;#39;s a psychic, Shawn opens a detective agency with best friend Burton Guster. &lt;/p&gt;\n&lt;/blockquote&gt;\n\n&lt;p&gt;If you love other USA shows as well, check out &lt;a href="/r/USANetwork"&gt;/r/USANetwork&lt;/a&gt; for the latest news and discussion about the network and its shows.&lt;/p&gt;\n\n&lt;p&gt;(This is not a community about psychology or psychiatry! Try &lt;a href="/r/psychology"&gt;/r/psychology&lt;/a&gt; or a related subreddit for that, or &lt;a href="/r/SampleSize"&gt;/r/SampleSize&lt;/a&gt; if looking to take or share surveys.)&lt;/p&gt;\n\n&lt;hr/&gt;\n\n&lt;ul&gt;\n&lt;li&gt;&lt;a href="http://www.usanetwork.com/psych/blog/heres-where-you-can-watch-every-episode-of-psych"&gt;A collection of official streaming and download links to watch every season of Psych&lt;/a&gt;&lt;/li&gt;\n&lt;/ul&gt;\n\n&lt;p&gt;If you miss that good ol&amp;#39; Shawn and Gus goodness, read through some old discussions from the last few seasons (linked below)!&lt;/p&gt;\n\n&lt;blockquote&gt;\n&lt;ul&gt;\n&lt;li&gt;&lt;strong&gt;Psych: The Movie Discussions&lt;/strong&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="https://www.reddit.com/r/psych/comments/7iao3w/s09e01_psych_the_movie_preshow_discussion_thread/"&gt;Pre-Show Discussion&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="https://www.reddit.com/r/psych/comments/7ib47u/s09e01_psych_the_movie_live_discussion_thread/"&gt;Live Discussion&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="https://www.reddit.com/r/psych/comments/7ibvb1/s09e01_psych_the_movie_postshow_discussion_thread/"&gt;Post-Show Discussion&lt;/a&gt;&lt;/li&gt;\n&lt;/ul&gt;\n&lt;/blockquote&gt;\n\n&lt;h2&gt;&lt;/h2&gt;\n\n&lt;blockquote&gt;\n&lt;ul&gt;\n&lt;li&gt;&lt;strong&gt;Season 8 Episode Discussions&lt;/strong&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1urh4a"&gt;Episode 1: &amp;quot;Lock, Stock, Some Smoking Barrels and Burton Guster&amp;#39;s Goblet of Fire&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1vbt49"&gt;Episode 2: &amp;quot;S.E.I.Z.E. the Day&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1vvadz"&gt;Episode 3: &amp;quot;Remake A.K.A. Cloudy... With a Chance of Improvement&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1wipzn"&gt;Episode 4: &amp;quot;Someone&amp;#39;s Got a Woody&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1x53iq"&gt;Episode 5: &amp;quot;Cog Blocked&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1z1r6h"&gt;Episode 6: &amp;quot;1967: A Psych Odyssey&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1zonq5"&gt;Episode 7: &amp;quot;Shawn and Gus Truck Things Up&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/20a4x1"&gt;Episode 8: &amp;quot;A Touch of Sweevil&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/20v2wy"&gt;Episode 9: &amp;quot;A Nightmare on State Street&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/21grej"&gt;Episode 10: &amp;quot;The Break-Up&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;/ul&gt;\n&lt;/blockquote&gt;\n\n&lt;h2&gt;&lt;/h2&gt;\n\n&lt;blockquote&gt;\n&lt;ul&gt;\n&lt;li&gt;&lt;strong&gt;Season 7 Episode Discussions&lt;/strong&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/19deg0"&gt;Episode 1: &amp;quot;Santabarbaratown 2&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/19ti91"&gt;Episode 2: &amp;quot;Juliet Takes a Luvvah&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1a9dh3"&gt;Episode 3: &amp;quot;Lassie Jerky&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1apfn0"&gt;Episode 4: &amp;quot;No Country for Two Old Men&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1b5fnt"&gt;Episode 5: &amp;quot;100 Clues&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1bms87"&gt;Episode 6: &amp;quot;Cirque du Soul&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1c3v1g"&gt;Episode 7: &amp;quot;Deez Nups&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1ckt2e"&gt;Episode 8: &amp;quot;Right Turn or Left for Dead&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1d1xi1"&gt;Episode 9: &amp;quot;Juliet Wears the Pantsuit&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1diw5f"&gt;Episode 10: &amp;quot;Santa Barbarian Candidate&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1dz2rg"&gt;Episode 11: &amp;quot;Office Space&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1efbjd"&gt;Episode 12: &amp;quot;Dead Air&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1evl42"&gt;Episode 13: &amp;quot;Nip and Suck It&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1fb2d8"&gt;Episode 14: &amp;quot;No Trout About It&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/1sz4is"&gt;Episode 15: &amp;quot;The Musical&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;/ul&gt;\n&lt;/blockquote&gt;\n\n&lt;h2&gt;&lt;/h2&gt;\n\n&lt;blockquote&gt;\n&lt;ul&gt;\n&lt;li&gt;&lt;strong&gt;Season 6 Episode Discussions&lt;/strong&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/lahle"&gt;Episode 1: &amp;quot;Shawn Rescues Darth Vader&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/li803"&gt;Episode 2: &amp;quot;Last Night Gus&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/lqgbg"&gt;Episode 3: &amp;quot;This Episode Sucks&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/lygnn"&gt;Episode 4: &amp;quot;The Amazing Psych-Man &amp;amp; Tap Man, Issue #2&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/m6wkq"&gt;Episode 5: &amp;quot;Dead Man&amp;#39;s Curve Ball&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/mf79p"&gt;Episode 6: &amp;quot;Shawn, Interrupted&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/mvlcz"&gt;Episode 7: &amp;quot;In For A Penny...&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/n41x5"&gt;Episode 8: &amp;quot;The Tao of Gus&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/nd93d"&gt;Episode 9: &amp;quot;Neil Simon&amp;#39;s Lover&amp;#39;s Retreat&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/qbuhl"&gt;Episode 10: &amp;quot;Indiana Shawn and the Temple of the Kinda Crappy, Rusty Old Dagger&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/qmrsg"&gt;Episode 11: &amp;quot;Heeeeere&amp;#39;s Lassie&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/qx5kr"&gt;Episode 12: &amp;quot;The Old and the Restless&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/r7umx"&gt;Episode 13: &amp;quot;Let&amp;#39;s Doo-Wop It Again&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/rij8s"&gt;Episode 14: &amp;quot;Autopsy Turvy&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/ru0bc"&gt;Episode 15: &amp;quot;True Grits&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="http://redd.it/s5jrw"&gt;Episode 16: &amp;quot;Santabarbaratown&amp;quot;&lt;/a&gt;&lt;/li&gt;\n&lt;/ul&gt;\n&lt;/blockquote&gt;\n\n&lt;ul&gt;\n&lt;li&gt;&lt;a href="http://www.reddit.com/r/psych/search?q=flair%3Arewatch&amp;amp;sort=new&amp;amp;restrict_sr=on"&gt;Series rewatch discussions&lt;/a&gt; (on hold)&lt;/li&gt;\n&lt;/ul&gt;\n\n&lt;hr/&gt;\n\n&lt;p&gt;&lt;strong&gt;Spoilers&lt;/strong&gt;&lt;/p&gt;\n\n&lt;p&gt;Typing: &lt;code&gt;[spoiler](#s &amp;quot;You know that&amp;#39;s right.&amp;quot;)&lt;/code&gt;&lt;/p&gt;\n\n&lt;p&gt;will show: &lt;a href="#s" title="You know that&amp;#39;s right"&gt;spoiler&lt;/a&gt;&lt;/p&gt;\n\n&lt;p&gt;Note: These do not work in titles...&lt;/p&gt;\n\n&lt;hr/&gt;\n\n&lt;p&gt;&lt;strong&gt;USA Network Show Subreddits&lt;/strong&gt;&lt;/p&gt;\n\n&lt;ul&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/BenchedTV"&gt;Benched&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/BurnNotice"&gt;Burn Notice&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/ComplicationsTV"&gt;Complications&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/CovertAffairs"&gt;Covert Affairs&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/DamnationTV"&gt;Damnation&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/FairlyLegal"&gt;Fairly Legal&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/FallingWaterTV"&gt;Falling Water&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/Monk"&gt;Monk&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/MrRobot"&gt;Mr. Robot&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/PlayingHouse"&gt;Playing House&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/PoliticalAnimals"&gt;Political Animals&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;strong&gt;Psych&lt;/strong&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/RoyalPains"&gt;Royal Pains&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/RushTV"&gt;Rush&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/SatisfactionTV"&gt;Satisfaction&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/Shooter"&gt;Shooter&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/SirensTV"&gt;Sirens&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/Suits"&gt;Suits&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;li&gt;&lt;p&gt;&lt;a href="/r/WhiteCollar"&gt;White Collar&lt;/a&gt;&lt;/p&gt;&lt;/li&gt;\n&lt;/ul&gt;\n\n&lt;p&gt;&lt;strong&gt;Recommended Subreddits&lt;/strong&gt;&lt;/p&gt;\n\n&lt;ul&gt;\n&lt;li&gt;&lt;a href="/r/PsychGifs"&gt;Psych Gifs&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="/r/maggielawson"&gt;Maggie Lawson&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="/r/community"&gt;Community&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="/r/numb3rs"&gt;Numb3rs&lt;/a&gt;&lt;/li&gt;\n&lt;li&gt;&lt;a href="/r/ArcherFX"&gt;Archer&lt;/a&gt;&lt;/li&gt;\n&lt;/ul&gt;\n&lt;/div&gt;&lt;!-- SC_ON --&gt;',
         'spoilers_enabled': True,
         'header_title': 'Are you a fan of delicious flavor?',
         'header_size': [120, 40],
         'user_flair_position': 'right',
         'all_original_content': False,
         'has_menu_widget': False,
         'is_enrolled_in_new_modmail': None,
         'key_color': '',
         'can_assign_user_flair': True,
         'created': 1240787080.0,
         'wls': 6,
         'show_media_preview': True,
         'submission_type': 'any',
         'user_is_subscriber': False,
         'disable_contributor_requests': False,
         'allow_videogifs': True,
         'user_flair_type': 'text',
         'allow_polls': False,
         'collapse_deleted_comments': False,
         'emojis_custom_size': None,
         'public_description_html': '&lt;!-- SC_OFF --&gt;&lt;div class="md"&gt;&lt;p&gt;A subreddit devoted to all things Psych. If you&amp;#39;re a fan of the silliest TV show on USA Network, slice open a pineapple and subscribe today!&lt;/p&gt;\n\n&lt;p&gt;(This is not a community about psychology or psychiatry! Try &lt;a href="/r/psychology"&gt;/r/psychology&lt;/a&gt; or a related subreddit for that, or &lt;a href="/r/SampleSize"&gt;/r/SampleSize&lt;/a&gt; if looking to take or share surveys.)&lt;/p&gt;\n&lt;/div&gt;&lt;!-- SC_ON --&gt;',
         'allow_videos': True,
         'is_crosspostable_subreddit': True,
         'suggested_comment_sort': None,
         'can_assign_link_flair': False,
         'accounts_active_is_fuzzed': False,
         'submit_text_label': '',
         'link_flair_position': 'left',
         'user_sr_flair_enabled': None,
         'user_flair_enabled_in_sr': False,
         'allow_discovery': True,
         'user_sr_theme_enabled': True,
         'link_flair_enabled': True,
         'subreddit_type': 'public',
         'notification_level': None,
         'banner_img': 'https://b.thumbs.redditmedia.com/gJtehqxMKfoZLfvQ6aFbxt-DZWooGTl0h9JUwBbz7BM.png',
         'user_flair_text': None,
         'banner_background_color': '',
         'show_media': True,
         'id': '2qxd2',
         'user_is_contributor': False,
         'over18': False,
         'description': '###*Psych: The Movie* [Pre-Show Discussion](https://www.reddit.com/r/psych/comments/7iao3w/s09e01_psych_the_movie_preshow_discussion_thread/) | [Live Discussion](https://www.reddit.com/r/psych/comments/7ib47u/s09e01_psych_the_movie_live_discussion_thread/) | [Post-Show Discussion](https://www.reddit.com/r/psych/comments/7ibvb1/s09e01_psych_the_movie_postshow_discussion_thread/)\n\n**A subreddit devoted to all things *Psych*!** This is the place to talk about the silliest, most pineapple-filled show on USA Network.\n\nShow Summary:\n\n&gt; Shawn Spencer has developed a keen eye for detail after being instructed by his police officer father to note even the most minute details of his surroundings. After conning the police into believing that he\'s a psychic, Shawn opens a detective agency with best friend Burton Guster. \n\nIf you love other USA shows as well, check out /r/USANetwork for the latest news and discussion about the network and its shows.\n\n(This is not a community about psychology or psychiatry! Try /r/psychology or a related subreddit for that, or /r/SampleSize if looking to take or share surveys.)\n\n***\n\n* [A collection of official streaming and download links to watch every season of Psych](http://www.usanetwork.com/psych/blog/heres-where-you-can-watch-every-episode-of-psych)\n\nIf you miss that good ol\' Shawn and Gus goodness, read through some old discussions from the last few seasons (linked below)!\n\n&gt;* **Psych: The Movie Discussions**\n* [Pre-Show Discussion](https://www.reddit.com/r/psych/comments/7iao3w/s09e01_psych_the_movie_preshow_discussion_thread/)\n* [Live Discussion](https://www.reddit.com/r/psych/comments/7ib47u/s09e01_psych_the_movie_live_discussion_thread/)\n* [Post-Show Discussion](https://www.reddit.com/r/psych/comments/7ibvb1/s09e01_psych_the_movie_postshow_discussion_thread/)\n\n-\n\n&gt;* **Season 8 Episode Discussions**\n* [Episode 1: "Lock, Stock, Some Smoking Barrels and Burton Guster\'s Goblet of Fire"](http://redd.it/1urh4a)\n* [Episode 2: "S.E.I.Z.E. the Day"](http://redd.it/1vbt49)\n* [Episode 3: "Remake A.K.A. Cloudy... With a Chance of Improvement"](http://redd.it/1vvadz)\n* [Episode 4: "Someone\'s Got a Woody"](http://redd.it/1wipzn)\n* [Episode 5: "Cog Blocked"](http://redd.it/1x53iq)\n* [Episode 6: "1967: A Psych Odyssey"](http://redd.it/1z1r6h)\n* [Episode 7: "Shawn and Gus Truck Things Up"](http://redd.it/1zonq5)\n* [Episode 8: "A Touch of Sweevil"](http://redd.it/20a4x1)\n* [Episode 9: "A Nightmare on State Street"](http://redd.it/20v2wy)\n* [Episode 10: "The Break-Up"](http://redd.it/21grej)\n\n-\n\n&gt;* **Season 7 Episode Discussions**\n* [Episode 1: "Santabarbaratown 2"](http://redd.it/19deg0)\n* [Episode 2: "Juliet Takes a Luvvah"](http://redd.it/19ti91)\n* [Episode 3: "Lassie Jerky"](http://redd.it/1a9dh3)\n* [Episode 4: "No Country for Two Old Men"](http://redd.it/1apfn0)\n* [Episode 5: "100 Clues"](http://redd.it/1b5fnt)\n* [Episode 6: "Cirque du Soul"](http://redd.it/1bms87)\n* [Episode 7: "Deez Nups"](http://redd.it/1c3v1g)\n* [Episode 8: "Right Turn or Left for Dead"](http://redd.it/1ckt2e)\n* [Episode 9: "Juliet Wears the Pantsuit"](http://redd.it/1d1xi1)\n* [Episode 10: "Santa Barbarian Candidate"](http://redd.it/1diw5f)\n* [Episode 11: "Office Space"](http://redd.it/1dz2rg)\n* [Episode 12: "Dead Air"](http://redd.it/1efbjd)\n* [Episode 13: "Nip and Suck It"](http://redd.it/1evl42)\n* [Episode 14: "No Trout About It"](http://redd.it/1fb2d8)\n* [Episode 15: "The Musical"](http://redd.it/1sz4is)\n\n-\n\n&gt;* **Season 6 Episode Discussions**\n* [Episode 1: "Shawn Rescues Darth Vader"](http://redd.it/lahle)\n* [Episode 2: "Last Night Gus"](http://redd.it/li803)\n* [Episode 3: "This Episode Sucks"](http://redd.it/lqgbg)\n* [Episode 4: "The Amazing Psych-Man &amp; Tap Man, Issue #2"](http://redd.it/lygnn)\n* [Episode 5: "Dead Man\'s Curve Ball"](http://redd.it/m6wkq)\n* [Episode 6: "Shawn, Interrupted"](http://redd.it/mf79p)\n* [Episode 7: "In For A Penny..."](http://redd.it/mvlcz)\n* [Episode 8: "The Tao of Gus"](http://redd.it/n41x5)\n* [Episode 9: "Neil Simon\'s Lover\'s Retreat"](http://redd.it/nd93d)\n* [Episode 10: "Indiana Shawn and the Temple of the Kinda Crappy, Rusty Old Dagger"](http://redd.it/qbuhl)\n* [Episode 11: "Heeeeere\'s Lassie"](http://redd.it/qmrsg)\n* [Episode 12: "The Old and the Restless"](http://redd.it/qx5kr)\n* [Episode 13: "Let\'s Doo-Wop It Again"](http://redd.it/r7umx)\n* [Episode 14: "Autopsy Turvy"](http://redd.it/rij8s)\n* [Episode 15: "True Grits"](http://redd.it/ru0bc)\n* [Episode 16: "Santabarbaratown"](http://redd.it/s5jrw)\n\n* [Series rewatch discussions](http://www.reddit.com/r/psych/search?q=flair%3Arewatch&amp;sort=new&amp;restrict_sr=on) (on hold)\n\n***\n\n**Spoilers**\n\nTyping: `[spoiler](#s "You know that\'s right.")`\n\nwill show: [spoiler](#s "You know that\'s right")\n\nNote: These do not work in titles...\n\n***\n\n**USA Network Show Subreddits**\n\n* [Benched](/r/BenchedTV)\n\n* [Burn Notice](/r/BurnNotice)\n\n* [Complications](/r/ComplicationsTV)\n\n* [Covert Affairs](/r/CovertAffairs)\n\n* [Damnation](/r/DamnationTV)\n\n* [Fairly Legal](/r/FairlyLegal)\n\n* [Falling Water](/r/FallingWaterTV)\n\n* [Monk](/r/Monk)\n\n* [Mr. Robot](/r/MrRobot)\n\n* [Playing House](/r/PlayingHouse)\n\n* [Political Animals](/r/PoliticalAnimals)\n\n* **Psych**\n\n* [Royal Pains](/r/RoyalPains)\n\n* [Rush](/r/RushTV)\n\n* [Satisfaction](/r/SatisfactionTV)\n\n* [Shooter](/r/Shooter)\n\n* [Sirens](/r/SirensTV)\n\n* [Suits](/r/Suits)\n\n* [White Collar](/r/WhiteCollar)\n\n**Recommended Subreddits**\n\n* [Psych Gifs](/r/PsychGifs)\n* [Maggie Lawson](/r/maggielawson)\n* [Community](/r/community)\n* [Numb3rs](/r/numb3rs)\n* [Archer](/r/ArcherFX)',
         'submit_link_label': '',
         'user_flair_text_color': None,
         'restrict_commenting': False,
         'user_flair_css_class': None,
         'allow_images': True,
         'lang': 'en',
         'whitelist_status': 'all_ads',
         'url': '/r/psych/',
         'created_utc': 1240758280.0,
         'banner_size': [900, 270],
         'mobile_banner_image': '',
         'user_is_moderator': False}}],
      'after': 't5_2qxd2',
      'before': None}}



So searching for "pineapple clues" led us to a subreddit on the tv show "Psych" which used to use hidden pineapples in every episode as an easter egg, which fans of the show now try and find in every episode. We got back information about that subreddit, such as description, its icon img, and title etc.

Here are some of the parameters that you could have passed in to this endpoint:

1. limit=[a number] which is the maximum number of enteries that you would like to be returned
2. q = [a string] the search query to search for the subreddits you are looking for
3. show_users = [a boolean] that would show all the users that have interacted with that subreddit and their username
4. sort = [an enum: "relevance" or "activity"] which would sort the results either by what reddit thought was the most relevant to the query or by which subreddits are currently experiencing the most activity

Alright. One more query. Now let's follow the above subreddit. More Api endpoints and parameters are listed here: https://www.reddit.com/dev/api/oauth#GET_api_live_happening_now


```python
params = dict(action="sub", sr_name=["psych"])
response = requests.post("https://oauth.reddit.com/api/subscribe", headers=headers, params=params)
response.json()
```




    {}



Here the parameters could have been:
1. action=[enum of either "sub" or "unsub"] depending of if you are trying to subscribe or unsubscribe
2. skip_initial_defaults=[boolean] if you want to or don't want to skip the initial defaults that come with subscribing to a subreddit
3. sr = [a comma separated list of fullnames] which takes a comma separated list of fullnames of the subreddits you want to subscribe to
OR
4. sr_name = [a comma separated list of subreddit name] which is a string that is the subreddit's name as shown in its url

Since the api didnt respond with an error, we are following it!

One thing to note is that the auth token will expire after an hour so if you are doing anything longer than that, you will have to renew your auth token.


```python

```
