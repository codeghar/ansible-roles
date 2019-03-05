# Jenkins

Install and configure Jenkins. Uses nginx as a reverse proxy and for TLS
termination.

Runs Jenkins and nginx reverse proxy in Docker containers using Docker Compose.
This makes it easier to pull and run latest versions of both while keeping the
data on the host.

The service is controlled by systemd.

Some initial configuration, including hardening of the install, is also
performed.

Installs these plugins:

- blueocean
- job-dsl
- pipeline
- role-strategy
- ssh-slaves

Make sure to change the password of the first admin user in the playbook.

This role was developed and tested against Ubuntu 18.04.

## Requirements

- Docker
- Docker Compose
- TLS certificate bundle and private key files must be created and placed in
the _files_ directory, named _certificate.pem_ and _key.pem_ respectively.

## Role Variables

- admin_user - First user to create in Jenkins. This user will have admin
rights. Default value is _admin_.
- admin_password - Password of the first user. Default value **must** be
changed as soon as possible.
- certificate_file - Name of the file to be created and put in _files_ directory
which contains the certificate bundle. Default value is _certificate.pem_.
- certificate_key_file - Name of file containing private key of the certificate.
Default value is _key.pem_.
- email - Email to configure in Jenkins. Default value is _not@configured.yet_.
- password - Initial [crypted](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module) password of the user. Default value is _IamInsecure_, which is not crypted on purpose.
- server_name - FQDN of the server. Default value is _localhost_.
- undo - Boolean whether to undo the changes made during install. Default value
is _false_.
- url - URL of the Jenkins leader server. Default value is _https://localhost/_.
- username - User to create for Jenkins leader to ssh as. Default value is
_jenkins_.
- validate_certs - Boolean value whether to verify TLS certificates. It is added
so it may override the default value, _yes_, in cases where privately signed
certificates are used.

## More Information

- https://technologyconversations.com/2017/06/16/automating-jenkins-docker-setup/
- https://wiki.jenkins.io/display/JENKINS/CSRF+Protection
- https://gist.github.com/peterjenkins1/8f8bdbc82669314f7a2cc392f48be6a0
- https://github.com/docker/compose/issues/4266#issuecomment-302813256
- https://wiki.jenkins.io/display/JENKINS/Jenkins+behind+an+NGinX+reverse+proxy
- https://www.thepolyglotdeveloper.com/2017/03/nginx-reverse-proxy-containerized-docker-applications/
- https://wiki.jenkins.io/display/JENKINS/Running+Jenkins+behind+Nginx

## Example Playbook

Run these required steps first:

- Install Docker on the host(s); you may use the _docker_ role from this repo
- Obtain TLS certificate and private key, for example from Let's Encrypt, and put them in _files/_ directory
- Generate crypted password

Obtain TLS certificate from Let's Encrypt. In this example I'm generating the certificate on a machine that is _not_
the web server and where my DNS records are managed in Digital Ocean. Your setup may vary so make appropriate
modifications.

    $ python3 -m pip install --user certbot
    $ python3 -m pip install --user certbot-dns-digitalocean
    $ mkdir -p workspace/{conf,logs,work}
    $ echo 'dns_digitalocean_token = YOUR_SECURE_TOKEN_HERE' > workspace/digitalocean.ini
    $ certbot certonly \
        --agree-tos \
        -m 'my@email.addr' \
        --config-dir workspace/conf \
        --logs-dir workspace/logs \
        --work-dir workspace/work \
        --dns-digitalocean \
        --dns-digitalocean-credentials workspace/digitalocean.ini \
        --dns-digitalocean-propagation-seconds 20 \
        -d jenkins.example.com
    $ cp workspace/conf/live/jenkins.example.com/fullchain.pem files/certificate.pem
    $ cp workspace/conf/live/jenkins.example.com/privkey.pem files/key.pem

More information on the above example,

- https://certbot.eff.org/docs/using.html#dns-plugins
- https://certbot-dns-digitalocean.readthedocs.io/en/stable/

Generate a [crypted password](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module),

    $ python3 -m pip install --user passlib
    $ python3 -c "from passlib.hash import sha512_crypt; import getpass; print(sha512_crypt.using(rounds=5000).hash(getpass.getpass()))"
    Password:
    $6$ebcDFuwHyR/D/SRP$r3nVDhepbjLEKkSkQxi2gDApd9Yitj3XRm1cUdTf88V0DIZCHnf22HRorSDund7xUlDeAXX8MJECDjSZ4ZOCD1

Now pass this password to the role,

    ---
    - hosts: "*"
      roles:
        - role: jenkins
          password: "$6$ebcDFuwHyR/D/SRP$r3nVDhepbjLEKkSkQxi2gDApd9Yitj3XRm1cUdTf88V0DIZCHnf22HRorSDund7xUlDeAXX8MJECDjSZ4ZOCD1"

Another example where we provide our environment information because the first example is not very usable out of the
box,

    ---
    - hosts: "*"
      roles:
        - role: jenkins
          password: "$6$ebcDFuwHyR/D/SRP$r3nVDhepbjLEKkSkQxi2gDApd9Yitj3XRm1cUdTf88V0DIZCHnf22HRorSDund7xUlDeAXX8MJECDjSZ4ZOCD1"
          admin_password: YOUR_PASSWORD_HERE
          url: "https://jenkins.example.com/"
          server_name: "jenkins.example.com"

To undo changes made in this role, set the variable _undo_ to _true_. It may not be a wholesale clean up, leaving some
bits behind. The reason is we want to be on the safe side most of the time.

    ---
    - hosts: "*"
      roles:
        - role: jenkins
          undo: true

## License

MIT
