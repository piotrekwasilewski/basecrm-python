# basecrm-python

BaseCRM Official API V2 library client for Python

## Installation

BaseCRM package can be installed either via pip or easy_install:

```bash
$ pip install --upgrade basecrm
```

or 

```bash
$ easy_install --upgrade basecrm
```

You can install from the source code as well. First clone the repo and then execute:

```bash
$ python setup.py install
```

After installing, import `basecrm` package:

```python
import basecrm
```

## Usage

```python
import basecrm

# Then we instantiate a client (as shown below)
```

### Build a client
__Using this api without authentication gives an error__

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
```

### Client Options

The following options are available while instantiating a client:

 * __access_token__: Personal access token
 * __base_url__: Base url for the api
 * __user_agent__: Default user-agent for all requests
 * __timeout__: Request timeout
 * __verbose__: Verbose/debug mode

### Architecture

The library follows few architectural principles you should understand before digging deeper.
1. Interactions with resources are done via service objects.
2. Service objects are exposed as properties on client instances.
3. Service objects expose resource-oriented actions.
4. Actions return dictionaries that support attribute-style access, a la JavaScript (thanks to Bunch).

For example, to interact with deals API you will use `basecrm.DealsService`, which you can get if you call:

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.deals # basecrm.DealsService
```

To retrieve list of resources and use filtering you will call `#list` method:

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.deals.list(organization_id=google.id, hot=True) # list(dict|Bunch)
```

To find a resource by it's unique identifier use `#retrieve` method:

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.deals.retrieve(id=google.id)
```

When you'd like to create a resource, or update it's attributes you want to use either `#create` or `#update` methods. For example if you want to create a new deal you will call:

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
deal = client.deals.create(name='Website redesign', contact_id=coffeeshop.id)

deal.value = 1000
deal.currency = 'USD'

client.deals.update(deal.id, deal)
```

To destroy a resource use `#destroy` method:

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.deals.destroy(coffeshopdeal.id)
```

There other non-CRUD operations supported as well. Please contact corresponding service files for in-depth documentation.

### Full example

Create a new organization and after that change it's attributes (website).

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
lead = client.leads.create(organization_name='Design service company')

lead.website = 'http://www.designservices.com'
client.leads.update(lead.id, lead)
```

### Error handling

When you instantiate a client or make any request via service objects, exceptions can be raised for multiple
of reasons e.g. a network error, an authentication error, an invalid param error etc. 

Sample below shows how to properly handle exceptions:

```python
try:
    # Instantiate a client.
    client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
    lead = client.leads.create(organization_name='Design service company')
    print lead
except basecrm.ConfigurationError as e:
    #  Invalid client configuration option
    pass
except basecrm.ResourceError as e:
    # Resource related error
    print 'Http status = ' + e.http_status
    print 'Request ID = ' + e.logref
    for error in e.errors: 
        print 'field = ' + error.field
        print 'code = ' + error.code
        print 'message = ' + error.message
        print 'details = ' + error.details
except basecrm.RequestError as e:
    # Invalid query parameters, authentication error etc.
    pass
except Exception as e:
    # Other kind of exceptioni, probably connectivity related
    pass
```

## Resources and actions

Documentation for every action can be found in `basecrm/services.py` file.

### Account

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.accounts # => basecrm.AccountsService
```

Actions:
* Retrieve account details - `client.accounts.self`

### AssociatedContact

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.associated_contacts # => basecrm.AssociatedContactsService
```

Actions:
* Retrieve deal's associated contacts - `client.associated_contacts.list`
* Create an associated contact - `client.associated_contacts.create`
* Remove an associated contact - `client.associated_contacts.destroy`

### Contact

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.contacts # => basecrm.ContactsService
```

Actions:
* Retrieve all contacts - `client.contacts.list`
* Create a contact - `client.contacts.create`
* Retrieve a single contact - `client.contacts.retrieve`
* Update a contact - `client.contacts.update`
* Delete a contact - `client.contacts.destroy`

### Deal

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.deals # => basecrm.DealsService
```

Actions:
* Retrieve all deals - `client.deals.list`
* Create a deal - `client.deals.create`
* Retrieve a single deal - `client.deals.retrieve`
* Update a deal - `client.deals.update`
* Delete a deal - `client.deals.destroy`

### Lead

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.leads # => basecrm.LeadsService
```

Actions:
* Retrieve all leads - `client.leads.list`
* Create a lead - `client.leads.create`
* Retrieve a single lead - `client.leads.retrieve`
* Update a lead - `client.leads.update`
* Delete a lead - `client.leads.destroy`

### LossReason

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.loss_reasons # => basecrm.LossReasonsService
```

Actions:
* Retrieve all reasons - `client.loss_reasons.list`
* Create a loss reason - `client.loss_reasons.create`
* Retrieve a single reason - `client.loss_reasons.retrieve`
* Update a loss reason - `client.loss_reasons.update`
* Delete a reason - `client.loss_reasons.destroy`

### Note

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.notes # => basecrm.NotesService
```

Actions:
* Retrieve all notes - `client.notes.list`
* Create a note - `client.notes.create`
* Retrieve a single note - `client.notes.retrieve`
* Update a note - `client.notes.update`
* Delete a note - `client.notes.destroy`

### Pipeline

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.pipelines # => basecrm.PipelinesService
```

Actions:
* Retrieve all pipelines - `client.pipelines.list`

### Source

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.sources # => basecrm.SourcesService
```

Actions:
* Retrieve all sources - `client.sources.list`
* Create a source - `client.sources.create`
* Retrieve a single source - `client.sources.retrieve`
* Update a source - `client.sources.update`
* Delete a source - `client.sources.destroy`

### Stage

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.stages # => basecrm.StagesService
```

Actions:
* Retrieve all stages - `client.stages.list`

### Tag

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.tags # => basecrm.TagsService
```

Actions:
* Retrieve all tags - `client.tags.list`
* Create a tag - `client.tags.create`
* Retrieve a single tag - `client.tags.retrieve`
* Update a tag - `client.tags.update`
* Delete a tag - `client.tags.destroy`

### Task

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.tasks # => basecrm.TasksService
```

Actions:
* Retrieve all tasks - `client.tasks.list`
* Create a task - `client.tasks.create`
* Retrieve a single task - `client.tasks.retrieve`
* Update a task - `client.tasks.update`
* Delete a task - `client.tasks.destroy`

### User

```python
client = basecrm.Client(access_token='<YOUR_PERSONAL_ACCESS_TOKEN>')
client.users # => basecrm.UsersService
```

Actions:
* Retrieve all users - `client.users.list`
* Retrieve a single user - `client.users.retrieve`
* Retrieve an authenticating user - `client.users.self`


## Tests

To run all test suites:

```bash
$ python setup.py test
```

And to run a single suite:

```bash
$ python setup.py test -s basecrm.test.test_associated_contacts_service.AssociatedContactsServiceTests
```

## Thanks

We would like to give huge thanks to [YunoJuno](https://www.yunojuno.com). They reqlinquished the package name
so we were able to publish official wrapper under __basecrm__ name.

Thank You!

## License
MIT

## Bug Reports
Report [here](https://github.com/basecrm/basecrm-python/issues).

## Contact
BaseCRM developers (developers@getbase.com)
