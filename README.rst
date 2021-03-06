===========================
OctoRest 
===========================

.. |CircleCI| image:: https://circleci.com/gh/dougbrion/OctoRest.svg?style=svg
    :target: https://circleci.com/gh/dougbrion/OctoRest

.. |License MIT| image:: https://img.shields.io/badge/License-MIT-yellow.svg
   :target: https://opensource.org/licenses/MIT

Python client library for OctoPrint REST API

This is continued work after the great start by Miro Hrončok of covering the
OctoPrint REST API. Nearly all current functionality in the API has been covered (up to OctoPrint 1.3.11),
but as of yet I have not had time to add extensive testing for all aspects of the API.

Installation
------------

The easiest way to install the package is via ``pip``::

    $ pip install octorest
    

Examples
--------

You may want a function which returns an instance of the OctoRest object connected to your printer.

.. code-block:: python

   def make_client(url, apikey):
        """Creates and returns an instance of the OctoRest client.
        
        Args:
            url - the url to the OctoPrint server
            apikey - the apikey from the OctoPrint server found in settings
        """
        try:
            client = OctoRest(url=url, apikey=apikey)
            return client
        except ConnectionError as ex:
            # Handle exception as you wish
            print(ex)
            
Once we have a client we can do many a cool thing with it!
For example the following retrieves all the G-code file names on your OctoPrint server and adds them to a string which is printed out.

.. code-block:: python

   def file_names(client):
        """Retrieves the G-code file names from the
        OctoPrint server and returns a string message listing the
        file names.
        
        Args:
            client - the OctoRest client
        """
        message = "The GCODE files currently on the printer are:\n\n"
        for k in client.files()['files']:
            message += k['name'] + "\n"
        print(message)

Maybe you want to stop your print and then subsequently home the printer. This is very simple to do using OctoRest!

.. code-block:: python

   def toggle_home(client):
        """Toggles the current print (if printing it pauses and
        if paused it starts printing) and then homes all of
        the printers axes.
        
        Args:
            client - the OctoRest client 
        """
        print("toggling the print!")
        client.toggle()
        print("Homing your 3d printer...")
        client.home()

Implemented features of OctoPrint REST API
------------------------------------------

A check list of the features currently implemented can be seen below.

* General information
    - [ ] Authorization
    - [ ] Login
    - [ ] Logout
* Version information
    - [X] Version information
* Apps
    - [ ] Session Keys (Deprecated since version 1.3.11: This functionality will be removed in 1.4.0. Use the Application Keys Plugin workflow instead.)
    
      - [ ] Obtaining a temporary session key
      - [ ] Verifying a temporary session key
* Connection handling
    - [X] Get connection settings
    - [X] Issue a connection command
    
      - [X] Connect
      - [X] Disconnect
      - [X] Fake_ack
* File operations
    - [X] Retrieve all files
    - [X] Retrieve files from specific location
    - [X] Upload file or create folder
    - [X] Retrieve a specific file’s or folder’s information
    - [X] Issue a file command
    
      - [X] Select
      - [X] Slice
      - [X] Copy
      - [X] Move
    - [X] Delete file
* Job operations
    - [X] Issue a job command
    
        - [X] Start
        - [X] Cancel
        - [X] Restart
        - [X] Pause
        
          - [X] Pause
          - [X] Resume
          - [X] Toggle
    - [X] Retrieve information about the current job
* Languages
    - [X] Retrieve installed language packs
    - [X] Upload a language pack
    - [X] Delete a language pack
* Log file management
    - [X] Retrieve a list of available log files
    - [X] Delete a specific logfile
* Printer operations
    - [X] Retrieve the current printer state
    - [X] Issue a print head command
    
      - [X] Jog
      - [X] Home
      - [X] Feedrate
    - [X] Issue a tool command
    
      - [X] Target
      - [X] Offset
      - [X] Select
      - [X] Extrude
      - [X] Flowrate
    - [X] Retrieve the current tool state
    - [X] Issue a bed command
    
      - [X] Target
      - [X] Offset
    - [X] Retrieve the current bed state
    - [X] Issue a chamber command
    
      - [X] Target
      - [X] Offset
    - [X] Retrieve the current chamber state
    - [X] Issue an SD command
    
      - [X] Init
      - [X] Refresh
      - [X] Release
    - [X] Retrieve the current SD state
    - [X] Send an arbitrary command to the printer
* Printer profile operations
    - [X] Retrieve all printer profiles
    - [ ] Add a new printer profile
    - [ ] Update an existing printer profile
    - [X] Remove an existing printer profile
* Settings
    - [X] Retrieve current settings
    - [X] Save settings
    - [ ] Regenerate the system wide API key
    - [ ] Fetch template data (in beta)
* Slicing
    - [X] List All Slicers and Slicing Profiles
    - [X] List Slicing Profiles of a Specific Slicer
    - [X] Retrieve Specific Profile
    - [ ] Add Slicing Profile
    - [X] Delete Slicing Profile
* System
    - [X] List all registered system commands
    - [X] List all registered system commands for a source
    - [X] Execute a registered system command
* Timelapse
    - [X] Retrieve a list of timelapses and the current config
    - [X] Delete a timelapse
    - [X] Issue a command for an unrendered timelapse
    
      - [X] Render
    - [X] Delete an unrendered timelapse
    - [X] Change current timelapse config
* User
    - [X] Retrieve a list of users
    - [X] Retrieve a user
    - [X] Add a user
    - [X] Update a user
    - [X] Delete a user
    - [X] Reset a user’s password
    - [X] Retrieve a user’s settings
    - [ ] Update a user’s settings
    - [X] Regenerate a user’s personal API key
    - [X] Delete a user’s personal API key
* Util
    - [X] Test paths or URLs
    
      - [X] Path
      - [X] URL
      - [X] Server
* Wizard
    - [X] Retrieve additional data about registered wizards
    - [X] Finish wizards

Copyright & License
-------------------

Copyright (c) 2016-2017 `Miro Hrončok <miro@hroncok.cz/>`_. MIT License.

Copyright (c) 2017 `Jiří Makarius <meadowfrey@gmail.com/>`_. MIT License.

Copyright (c) 2018-2019, `Douglas Brion <me@douglasbrion.com/>`_. MIT License.
