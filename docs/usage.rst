Usage
=====

Reading WebVTT caption files
----------------------------

.. code-block:: python

    from webvtt import WebVTT

    webvtt = WebVTT().read('captions.vtt')

    # we can iterate over the captions
    for caption in webvtt:
        print(caption.start)  # start timestamp in text format
        print(caption.end)  # end timestamp in text format
        print(caption.text)  # caption text

    # you can also iterate over the lines of a particular caption
    for line in webvtt[0].lines:
        print(line)

    # caption text is returned clean without class tags
    # we can access the raw text of a caption with raw_text
    >>> webvtt[0].text
    'This is a caption text'
    >>> webvtt[0].raw_text
    'This is a <c.colorE5E5E5>caption</c> text'

    # caption identifiers
    >>> webvtt[0].identifier
    'crédit de transcription'


Creating captions
-----------------

.. code-block:: python

    from webvtt import WebVTT, Caption

    webvtt = WebVTT()

    # creating a caption with a list of lines
    caption = Caption(
        '00:00:00.500',
        '00:00:07.000',
        ['Caption line 1', 'Caption line 2']
    )

    # adding a caption
    webvtt.captions.append(caption)

    # creating another caption with a text
    caption = Caption(
        '00:00:07.000',
        '00:00:11.890',
        'Caption line 1\nCaption line 2']
    )

    webvtt.captions.append(caption)


Manipulating captions
---------------------

.. code-block:: python

    from webvtt import WebVTT

    webvtt = WebVTT().read('captions.vtt')

    # update start timestamp
    webvtt[0].start = '00:00:01.250'

    # update end timestamp
    webvtt[0].end = '00:00:03.890'

    # update caption text
    webvtt[0].text = 'My caption text'

    # delete a caption
    del webvtt.captions[2]


Saving captions
---------------

.. code-block:: python

    from webvtt import WebVTT

    webvtt = WebVTT().read('captions.vtt')

    # save to original file
    webvtt.save()

    # save to a different file
    webvtt.save('my_captions.vtt')

    # write to opened file
    with open('my_captions.vtt', 'w') as fd:
        webvtt.write(fd)


Converting captions
-------------------

You can read captions from the following formats:

* SubRip (.srt)
* YouTube SBV (.sbv)

.. code-block:: python

    from webvtt import WebVTT

    # to read from a different format use the method from_ followed by
    # the extension.
    webvtt = WebVTT().from_sbv('captions.sbv')
    webvtt.save()

    # if we just want to convert the file we can do this in one line
    WebVTT().from_sbv('captions.sbv').save()

Also we can convert to other formats:

* SubRip (.srt)

.. code-block:: python

    from webvtt import WebVTT

    # save in SRT format
    webvtt = WebVTT().read('captions.vtt')
    webvtt.save_as_srt()

    # write to opened file in SRT format
    with open('my_captions.srt', 'w') as fd:
        webvtt.write(fd, format='srt)
