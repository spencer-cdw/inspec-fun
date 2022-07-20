# Inspec-fun

Proof of concept with inspec

## Resources

- [Learn Inspec](https://learn.chef.io/courses/course-v1:chef+Inspec101+Perpetual/courseware/f0c24b8d4c37441d948720fce775d485/d0135397fd794a848168d454e8f0f079/?child=first)


`inspec-shell` has an inventory of all objects on the filesystem and attributes for that object (similar to puppet hiera or chef ohai or ansible facts)

```
inspec shell -t ssh://root:password@target

file('/tmp').class.superclass.instance_methods(false).sort
=> [:allowed?,
 :basename,
 :block_device?,
 :character_device?,
 :contain,
 :content,
 :directory?,
 :executable?,
 :exist?,
 :file,
 :file?,
 .....
```

inspec can run against any host and doesn't require any agents to be installed. As long as ssh/winrm are available inspec can inspect. 

There are community standards for hardening. Inspec can inherit from those baselines. You can wrap community baselines with your own customizations

- https://github.com/dev-sec/linux-baseline/
- https://github.com/inspec/inspec/tree/main/examples/profile



## Examples

Ensure nginx > 1.10.3 and it has 3 modules installed
```
control 'nginx-version' do
  impact 1.0
  title 'NGINX version'
  desc 'The required version of NGINX should be installed.'
  describe nginx do
    its('version') { should cmp >= '1.10.3' }
  end
end

control 'nginx-modules' do
  impact 1.0
  title 'NGINX version'
  desc 'The required NGINX modules should be installed.'
  describe nginx do
    its('modules') { should include 'http_ssl' }
    its('modules') { should include 'stream_ssl' }
    its('modules') { should include 'mail_ssl' }
  end
```

### Usage

You can call inspec against any winrm/ssh host

```
inspec exec /root/my_nginx -t ssh://root:password@target
```