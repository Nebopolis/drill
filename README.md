# Woodpecker 🐦

Woodpecker is a HTTP load testing application written in Rust. The main goal
for this project is to build a really lightweight tool as alternative to other
that require JVM and other stuff.

You can write brenchmark files, in YAML format, describing all the stuff you
want to test. You

It was inspered by [Ansible](http://docs.ansible.com/ansible/playbooks_intro.html)
syntax because it is really easy to use and extend.

Here is an example for **config.yml**:

```yaml
---

threads: 4
base_url: 'http://localhost:9000'
iterations: 5
```

Here is an example for **benchmark.yml**:

```yaml
---

- name: Fetch users
  request:
    url: /api/users.json

- name: Fetch account
  request:
    url: /api/account
  assign: foo

- name: Fetch last user
  request:
    url: /api/users/{{ foo.body.id }}

- name: Fetch some users
  request:
    url: /api/users/{{ item }}
  with_items:
    - 70
    - 73
    - 75

- name: Fetch some users by hash
  request:
    url: /api/users/{{ item.id }}
  with_items:
    - { id: 70 }
    - { id: 73 }
    - { id: 75 }

- name: Fetch some users from CSV
  request:
    url: /api/users/contacts/{{ item.id }}
  with_items_from_csv: ./fixtures/users.csv
```

As you can see you can play with interpolations in different ways. This
will let you specify a benchmank with different request and
dependencies between them.

## Install

The easiest way right now is to download the source code and compile
it:

```
git clone git@github.com:fcsonline/woodpecker.git && cd woodpecker
cargo build --release
./target/release/woodpecker
```

## Roadmap

- Complete and improve the interpolation engine
- Add cookie support
- Add writing to a file support
- Add statistic support

## Contribute

This project started as a side project to learn Rust, so I'm sure that is full
of mistakes and areas to be improve. If you think you can tweak the code to
make it better, I'll really appreaciate a pull request. ;)

