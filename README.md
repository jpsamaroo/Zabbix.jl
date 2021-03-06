[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Build Status](https://travis-ci.org/rahulkp220/Zabbix.jl.svg?branch=master)](https://travis-ci.org/rahulkp220/Zabbix.jl)
[![codecov.io](http://codecov.io/github/rahulkp220/Zabbix.jl/coverage.svg?branch=master)](http://codecov.io/github/rahulkp220/Zabbix.jl?branch=master)
[![GitHub contributors](https://img.shields.io/github/contributors/rahulkp220/Zabbix.jl.svg)](https://github.com/rahulkp220/Zabbix.jl/graphs/contributors)
[![GitHub issues](https://img.shields.io/github/issues/rahulkp220/Zabbix.jl.svg)](https://github.com/rahulkp220/Zabbix.jl/issues/)
[![GitHub version](https://badge.fury.io/gh/rahulkp220%2FZabbix.jl.svg)](https://github.com/rahulkp220/Zabbix.jl)

[![ForTheBadge built-by-developers](http://ForTheBadge.com/images/badges/built-by-developers.svg)](https://GitHub.com/rahulkp220/)

# Zabbix.jl
Julia bindings for Zabbix API :fire:

The package has been tested over version `3.2.11`.

## Installation
Since `Zabbix` is registered in `METADATA.jl`, you can directly install it like,
```julia
julia> Pkg.add("Zabbix")
```
## Update
```julia
julia> Pkg.update("Zabbix")
```

## How it works?
Please make sure that you have a valid Zabbix URL available along with the right access levels. In case you have only `read` access on the zabbix server, you may not be able to use configuration and management operations. Full `read & write` access is desired.

#### Creating a ZabbixAPI instance

```julia
julia> zabbix = Zabbix.ZabbixAPI("http://SERVER_IP/zabbix/api_jsonrpc.php","USERNAME","******",false)
Zabbix.ZabbixAPI("http://SERVER_IP/zabbix/api_jsonrpc.php", "USERNAME","******",false, 0, Dict("Content-Type"=>"application/json-rpc"), "2.0")
```
Note that I have set `verbose=false`. However, by default we have, `verbose=true`. 
Hence only set `verbose=false` if you are okay continuing without the info messages.

#### Get the Zabbix API's version Info
```julia
julia> Zabbix.api_version(zabbix)
v"3.2.11"

julia>typeof(Zabbix.api_version(zabbix))
VersionNumber
```

#### Get the auth token
```julia
julia> Zabbix.auth_token(zabbix)
"e8f8354d66f7fac2691f5c7441b8dfa0"
```

#### Get all hosts for a user
```julia
julia> Zabbix.get_all_hosts(zabbix)
Dict{String,Any} with 3 entries:
  "id"      => 1
  "jsonrpc" => "2.0"
  "result"  => Any[Dict{String,Any}(Pair{String,Any}("host", "localhost"),Pair{String,Any}("interfaces", Any[Dict{String,Any}(Pair{String,An…
```

#### Make any request to the zabbix server

The `make_request` function requires you to pass `methods`(aka Zabbix methods like `hosts.get` etc) and `params` ie.
parameters in a form of a `Dict()` object. A easy sample is given on Zabbix's official [website](https://www.zabbix.com/documentation/2.2/manual/api)
```julia

# another way to get the zabbix version
julia> Zabbix.make_request(zabbix, "apiinfo.version", Dict())
v"3.2.11"

# getting the details of a host given its hostname
julia> method = "host.get"
"host.get"

julia> params = Dict("output"=>"extend", "filter"=>Dict("host"=>["localhost"]))
Dict{String,Any} with 2 entries:
  "output" => "extend"
  "filter" => Dict("host"=>String["localhost"])

julia> Zabbix.make_request(zabbix, method, params)
Dict{String,Any} with 3 entries:
  "id"      => 1
  "jsonrpc" => "2.0"
  "result"  => Any[Dict{String,Any}(Pair{String,Any}("lastaccess", "0"),Pair{String,Any}("ipmi_privilege", "2"),Pair{String,Any}("ipmi_error…

 julia> Zabbix.make_request(zabbix, method, params)["result"][1]
Dict{String,Any} with 39 entries:
  "lastaccess"         => "0"
  "ipmi_privilege"     => "2"
  "ipmi_errors_from"   => "0"
  "snmp_available"     => "0"
  "templateid"         => "0"
  "disable_until"      => "0"
  "jmx_available"      => "0"
  "maintenance_from"   => "0"
  "tls_psk_identity"   => ""
  "available"          => "1"
  "ipmi_password"      => ""
  "tls_accept"         => "1"
  "name"               => "localhost"
  "tls_issuer"         => ""
  "status"             => "0"
  "maintenance_status" => "0"
  "hostid"             => "10084"
  "tls_connect"        => "1"
  "ipmi_available"     => "0"
  "description"        => ""
  "errors_from"        => "0"
  "maintenance_type"   => "0"
  "error"              => ""
  "ipmi_username"      => ""
  "snmp_disable_until" => "0"
  "snmp_error"         => ""
  "tls_subject"        => ""
  "maintenanceid"      => "0"
  "host"               => "localhost"
  "jmx_error"          => ""
  "ipmi_disable_until" => "0"
  "snmp_errors_from"   => "0"
  ⋮                    => ⋮

julia> Zabbix.make_request(zabbix, method, params)["result"][1]["hostid"]
"10084"
```

## Facing issues? :scream:
* Open a PR with the detailed expaination of the issue
* Reach me out [here](https://www.rahullakhanpal.in)
