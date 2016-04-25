pynder
======

This is a python client for the `Tinder <http://gotinder.com>`_ API.

Please see the examples for more information on how to use.

You start by instantiating a pynder.Session object with a Facebook ID and
Facebook access token.

You can get a temporary access token by going `HERE <https://www.facebook.com/dialog/oauth?client_id=464891386855067&redirect_uri=https://www.facebook.com/connect/login_success.html&scope=basic_info,email,public_profile,user_about_me,user_activities,user_birthday,user_education_history,user_friends,user_interests,user_likes,user_location,user_photos,user_relationship_details&response_type=token>`_ url and retrieving the token from the location header in the response. You can use the packet sniffer in Chrome devtools to do this. A longer-lasting token can be obtained by using `Charles <https://www.charlesproxy.com/>`_ as explained in this `article on how to create a DJ Khaled tinder bot <http://www.joelotter.com/2015/05/17/dj-khaled-tinder-bot.html>`_.



Once your session is initialized you have the following methods / attributes:
::

    import pynder
    session = pynder.Session(facebook_id, facebook_auth_token)
    session.matches() # get users you have already been matched with
    session.update_location(LAT, LON) # updates latitude and longitude for your profile
    session.profile  # your profile. If you update its attributes they will be updated on Tinder.
    users = session.nearby_users() # returns a list of users nearby

When you run nearby_users you will receive a list of `Hopeful` objects.
These have the following properties: ::

    user = users[0]
    user.bio # their biography
    user.name # their name
    user.photos # a list of photo URLs
    user.thumbnail #a list of thumbnails of photo URLS
    user.age # their age
    user.birth_date # their birth_date
    user.ping_time # last online
    user.distance_km # distance from you
    user.common_connections # friends in common
    user.common_interests # likes in common - returns a list of {'name':NAME, 'id':ID}
    user.get_photos(width=WIDTH) # a list of photo URLS with either of these widths ["84","172","320","640"]
    user.instagram_username # instagram username
    user.instagram_photos # a list of instagram photos with these fields for each photo: 'image','link','thumbnail'
    user.schools # list of schools
    user.jobs # list of jobs

You may run `user.like()`, `user.superlike()` or `user.dislike()` on that user.

For your list of matches, they will have a .user attribute with the same attributes as above except
you can't dislike or like them. You can, however, see any messages exchanged
(`match.messages`) or send them a message yourself
(`match.message("Eyyyy gurl")`).

One important thing to note is that every time you run the nearby_users function, you will exhaust those users as potential matches. You may want to persist those hopefuls to a database like MongoDB or SQLite if you want to be able to like or dislike them at a later time.

Please let me know if you have any questions or bug reports.

Testing
-------

To run the tests add a ``test.ini`` to the ``pynder/tests/`` folder with your
facebook auth details::

    [FacebookAuth]
    facebook_id = XXXX
    facebook_token = YYYY

And install the needed test deps::

    $ pip install vcrpy nose coverage

Now we we can run the tests::

    $ nosetests pynder --with-coverage --cover-package pynder

**ATTENTION** The recored request may contain personal data so remove them
before committing.
