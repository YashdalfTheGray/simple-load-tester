# simple-load-tester

A simple load tester written in Ruby with Docker and AWS Fargate powers 🚀

## What is this?

`simple-load-tester` is a relatively simple Ruby program that can be used to load test your application. Since it's so simple, it can scale in many exciting ways and to further help the scaling, there is also a `Dockerfile` included that is ready to go and builds into a 54mb image.

## How do I use it?

There are a couple of ways to use this project - by cloning it down and running the load tester using the Ruby version installed on your system (Ruby version 2.6 recommended because it's the only one this has been tested against) or by running it under Docker at which point, you don't have to worry about what Ruby version you need to use.

## I'm here for AWS Fargate

First off, 💯

This repository comes with a CloudFormation template called `load-test-with-fargate.yml` that you can use to spin up this simple load test in a Docker container on Amazon ECS using capacity from AWS Fargate! This means, no having to manage your instances and trying to register them with the ECS cluster. All of that happene through Fargate!

A couple of caveats, this will probably not be free, it's going to cost some money for you to load test stuff using AWS Fargate but I imagine that these load tests won't run all month so the cost shouldn't be in the large number ranges. Secondly, since this is load test only, it uses public subnets and attaches public IP addresses to each Fargate task that is spun up. If you want private tasks, you're going to need to spin up private subnets, use a NAT Gateway and some Private Link endpoints to make the whole thing work.

The only requirement is that you need to be in a region that has fargate and you need to have finished the ECS first run experience that uses Fargate to run something on ECS. This is mostly because the ECS console would have created the role that this template depends on to exist.

## Okay, but, how do I really use it?

### The quick Docker way

To run this under Docker, just run `docker run -it --name load-test yashdalfthegray/simple-load-tester --url <some_url>`. This will pull down the image from DockerHub and run it to load test `some_url` 1000 times with a 0.25 second wait time between each request.

### The _other_ Docker way

You can pull down this project, build your own image from it and then run it under Docker as well if you wanted. Run the following commands to do so, after cloning the code down. This can help if you want to make changes to the project.

```shell
docker build -t simple-load-tester .
docker run -it --name load-test simple-load-tester --url <some_url>
```

### The non-Docker way

You can clone down this package, run `./load-test --url <some_url>` to invoke your system's Ruby and load test that way. Sure...but...Docker! This usecase is still valid though because sometimes, you just don't have access to Docker.

## How do I configure this thing?

There are a couple of configuration options. They will display when your run either a Docker container or the executable itself with the `--help` or `-h` flag. The output of that is below with all the configuration options.

```shell
Usage: example.rb [options]

    -u, --url URL                    required, the URL to load test
    -t, --times TIMES                the number of times to curl the url
    -s, --sleep SLEEP                the time in seconds (can be fractional) to sleep between requests
    -h, --help                       prints this help message
    -v, --verbose                    run in verbose mode
```

## You said something about scaling...

Yeah, this project can scale pretty well since it runs under Docker. To account for the fact that multiple copies of this might run at the same time and target the same URL, the executable waits a random amount of time in the interval of [0.0, 1.0) seconds. You should be able to run this on a single computer, on multiple computers, on a cloud VM, on Amazon ECS, wherever and if you've got the resources, you can keep spawning copies of this until you run out of them.

If you run into a usecase that doesn't work, cut an issue and I'll be happy to look into it.

## How do I contribute?

If you run into issues or think of something that this could potentially benefit from, create an issue. I'd like to keep this project fairly simple but that doesn't mean that features aren't worth adding to it.

If you're feeling really adventurous, make some changes and send in a PR! They're always appreciated. Make sure your code is clean and you've atleast run it through Rubocop. Also make sure that you maintain compatibility with atleast Ruby 2.6 and your changes still work under Docker.

## License

MIT
