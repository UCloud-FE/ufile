
# Download and configure for use

## Source Code Download

* Download source code

```
git clone https://github.com/us3-epoch/ClickHouse
```

* Cut to the specified branch

```
git checkout us3_support_v20.8.7.15-lts
```

* Download the dependent submodules

```
git submodule update --init --recursive
```

## Compile

The clickhouse compilation relies on gcc/llvm, cmake, ninja. if you use gcc, make sure the version is 10 and above. You can prepare the compilation environment according to [official instructions](https://clickhouse.tech/docs/en/development/build/).

Compilation.

```bash
cd ClickHouse
mkdir build
cd build
cmake ...
ninja
```

*Note: If you are using gcc, replace `cmake ... ` replace `cmake -DENABLE_EMBEDDED_COMPILER=0 -DUSE_INTERNAL_LLVM_LIBRARY=0 -DWERROR=0 . `

## Configuration and Usage

If you need to use us3 as backend storage, you need to add disk configuration to the configuration file. Please refer to [official link](https://clickhouse.tech/docs/en/operations/server-configuration-parameters/settings/) for detailed configuration file settings

Add the following configuration to the disks of the configuration file.

```xml
        <disks>
            <your_name>
                <type>us3</type>
                <endpoint>ufile.cn-north-02.ucloud.cn</endpoint
                <bucket>your-bucket</bucket>
                <access_key>***************</access_key>
                <secret_key>***************</secret_key>
                <prefix>/clickhouse/</prefix>
            </your_name>
        </disks>
```

Add the following configuration to the policies:

```xml
        <policies>
            <your_name>
                <volumes>
                    <main>
                        <disk>your_disk_name</disk
                    </main>
                </volumes>
            </your_name>
        </policies>
```

Add the following statement when creating the table

```sql
SETTINGS your_setting,
storage_policy = 'your-policy-name';
```

That creates the table using us3 as the storage backend.

You can check whether the policy is created successfully with the following command.

```
clickhouse-client

select * from system.storage_policies
```

The output is as follows (some parts are omitted)

```
┌─policy_name─┬─volume_name─┬─volume_priority─┬─disks────────────┬─volume_type─┬─max_data_part_size─┬─move_factor─┐
│ default │ default │ 1 │ ['default'] │ JBOD │ 0 │ 0 │
│ testpolicy │ main │ 1 │ ['testdiskname'] │ JBOD │ 0 │ 0.1 │
└─────────────┴─────────────┴─────────────────┴──────────────────┴─────────────┴────────────────────┴─────────────┘
```
