fuel-jobqueue
=============

Jobqueue package for FuelPHP.


## Requirements ##

* PHP 5.3.3+
* [symfony/Process](https://github.com/symfony/Process) 2.3
* [pda/pheanstalk](https://github.com/pda/pheanstalk)
* [beanstalkd](http://kr.github.io/beanstalkd/)


## Installation ##

### A: Using composer ###

Add required package to your `composer.json`.

```json
    ...
    "require": {
        ...
        "hosopy/fuel-jobqueue": "dev-master",
        ...
    },
    ...
```

And run composer installer.

```
$ php composer.phar install
```

### B: Manual ###

Download zip archive or clone this repository, and extract to `fuel/app/packages/fuel-jobqueue`.


## Usage ##

### 1. Enable package ###

```php
'always_load' => array(
    'packages' => array(
        'fuel-jobqueue',
        ...
```


### 2. Configuration ###

Copy default configuration file to your app/config directory.

```
$ cp fuel/packages/fuel-jobqueue/config/jobqueue.php fuel/app/config
```

If you want to run beanstalkd in other machine or port, edit configuration.

For the moment, keep default configuration.

```php
return array(
    // default connection name
    'default' => 'default_connection',
    
    'connections' => array(
        'default_connection' => array(
            'driver'   => 'beanstalkd',
            'host'     => '127.0.0.1',
            'port'     => '11300',
            'queue'    => 'jobqueue',
        ),
    ),
);
```


### 3. Install and run beanstalkd ###

Currently, fuel-jobqueue uses [beanstalkd](http://kr.github.io/beanstalkd/) as a backend of queueing.

If beanstalkd is not installed in your machine, install it first.

```
# Example: homebrew (Mac)
$ brew install beanstalkd
$ beanstalkd
```

```
# Example: Ubuntu
$ sudo apt-get install beanstalkd
```

If you want to know about beanstalkd more, see [](https://github.com/kr/beanstalkd).


### 4. Define Job ###

Define your job handler class in `fuel/app/classes/myjob.php`.

```php
<?php
class Myjob
{
    // [IMPORTANT] Requires 'fire' method as a entry point.
    public function fire($job, $data)
    {
        // heavy job
        sleep(10);
    }
}
```

### 5. Queueing Job ###

In your controller, call `\Jobqueue\Queue::push($job, $data)` to push a new job.

```php
class Controller_Welcome extends Controller
{
    public function action_index()
    {
        // push a new job onto the default queue of the default connection.
        // 'Myjob' is a class name you have defined.
        \Jobqueue\Queue::push('Myjob', array('message' => 'Hello World!'));
        
        return Response::forge(View::forge('welcome/index'));
    }
    ...
```

### 6. Run worker task ###

Queued jobs cannot be executed untill the worker process pop it from the queue.

```
$ cd FUEL_ROOT
$ php oil refine jqworker:listen --connection=default_connection --queue=jobqueue
```

### 7. Execute controller action ###

Now, we are ready for queueing!

Execute your controller action.


## Configuration ##

Sorry under construction...


## Job ##

Sorry under construction...


## Queue ##

Sorry under construction...


## Task ##

Sorry under construction...
