Orator Validator
################

This is an orator plugin that you can use to validate
your model when the user is creating a new item on the
database is easy to use and cleans the code a lot

Installation
============

You can install the plugin by using pip

.. code-block:: bash

  pip install orator-model-validator


How to use it
=============

this is and example of how to implementend on your code


.. code-block:: python

  from orator_validator.validator import Validator
  from orator import Model


  class User(Model, Validator):

      __connection__ = 'local'
      __fillable__ = [
            'name', 'email', 'password', 'phone_number'
      ]
      __guarded__ = ['id', 'password']


  class UserValidation(object):

      def saving(self, user):
          user.validate('name', require=True)
          user.validate(
              'email', regex="(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|\"(?:[\\x01-\\x08\\x0b\\x0c\\x0e-\\x1f\\x21\\x23-\\x5b\\x5d-\\x7f]|\\[\\x01-\\x09\\x0b\\x0c\\x0e-\\x7f])*\")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\\[(?:(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9]))\\.){3}(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9])|[a-z0-9-]*[a-z0-9]:(?:[\\x01-\\x08\\x0b\\x0c\\x0e-\\x1f\\x21-\\x5a\\x53-\\x7f]|\\[\\x01-\\x09\\x0b\\x0c\\x0e-\\x7f])+)\\])"
          )
          user.validate(
              'password', regex="^(?=.*[A-Za-z])(?=.*\\d)(?=.*[@$!%*#?&])[A-Za-z\\d@$!%*#?&]{6,}$"
          )
          user.errors()

  User.observe(UserValidation())


the validate function accept this params

* **require:** boolean when True checks if they send the value
* **data_type:** string Verifies if the value is specific data type
* **regex:** string pass a regex to verified
* **date_str:** string witch you want to check the format of the date example '%H:%M'