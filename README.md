# config
configuration management experiment written in typescript

## Example Usage
```
import * as path from 'path';
import {ConfigLoader} from '../src/Config/src/ConfigLoader';

var basePath = path.join(__dirname, '..', 'config');
var extension1BasePath = path.join(__dirname, '../extensions/extension1/config');
var extension1OverridePath = path.join(basePath, 'extension1');
var extension2BasePath = path.join(__dirname, '../extensions/extension2/config');
var extension2OverridePath = path.join(basePath, 'extension2');

var config = new ConfigLoader(basePath);
config.registerNamespace('extension1', extension1BasePath, extension1OverridePath);
config.registerNamespace('extension2', extension2BasePath, extension2OverridePath);

config.get('default.configKey');
config.get('extension1:default.configKey');
config.get('extension1:path1.configKey');
config.get('extension1:path1.configKey.configSubKey');
config.get('extension1:path1/path2.configKey');
config.get('default.first.second.third.forth');
config.get('extension1:default.first.second.third.forth');
config.get('extension1:path_a/path_b/path_c.first.second.third.forth');
config.get('extension2:default.config', 'TEST');
```

## Config key path
```
config.get([namespace:][paths.]file.key, default);
```

For the above examples:


| Namespace  | Paths         | File    | Key                       | Default |
|------------|---------------|---------|---------------------------|---------|
| -          | -             | default | configKey                 | -       |
| extension1 | -             | default | configKey                 | -       |
| extension1 | -             | path1   | configKey                 | -       |
| extension1 | -             | path1   | configKey.configSubKey    | -       |
| extension1 | path1         | path2   | configKey                 | -       |
|  -         | -             | default | first.second.third.forth  | -       |
| extension1 | -             | default | first.second.third.forth  | -       |
| extension1 | path_a/path_b | path_c  | first.second.third.forth  | -       |
| extension2 | -             | default | config                    | TEST    |

## Paths
You can add two paths per namespace. One for default configuration and one for the overrides. In addition you can add environment configurations in the override paths.

This may be a config file hierarchy:

```
// NODE_ENV = prod
var config = new ConfigLoader('config', 'override');
config.get('default.someKey');
```

- config/default.ts - (First level)
- override/default.ts - (Second level)
- override/default.prod.ts - (Third level)

```
// NODE_ENV = dev
var config = new ConfigLoader('config', 'override');
config.get('somePath/default.someKey');
```

- config/somePath/default.ts - (First level)
- override/somePath/default.ts - (Second level)
- override/somePath/default.dev.ts - (Third level)

## Configuration file
```
export default {
	someKey: 'value'
}
```
