# Permissions

## Description

Retrieves access [permissions](../../../administration/user-authorization.md) for the current user.

## Request

| **Method** | **Path** |
|:---|:---|
| GET | `/api/v1/permissions` |

## Response

### Fields

| **Name** | **Type** | **Description** |
|:---|:---|:---|
| `roles`  | array | List of user [roles](../../../administration/user-authorization.md#api-roles). |
| `user-groups` | array | List of groups to which the user belongs.|
| `entity-groups` | map | The user's entity groups `READ` and `WRITE` [permissions](../../../administration/user-authorization.md#entity-permissions), for example `"aws-ec2": "WRITE"`. |
| `portals`  | array | List of portals the user is authorized to view.|
| `all-entities-read` | boolean | User is authorized to read data for any entity.|
| `all-entities-write`  | boolean | User is authorized to write data for any entity.|
| `all-portals-permission`  | boolean | User is authorized to view all portals enabled in the system. |

## Example

### Request

#### URI

```elm
GET /api/v1/permissions
```

#### curl

```bash
curl https://atsd_hostname:8443/api/v1/permissions \
  --insecure --include --user {username}:{password}
```

### Response

* Non-admin

```json
{
  "roles": [
    "ROLE_API_DATA_WRITE", "ROLE_API_DATA_READ", "ROLE_API_META_WRITE", "ROLE_API_META_READ"
  ],
  "user-groups": [
    "Editors"
  ],
  "entity-groups": {
    "java-virtual-machine": "READ"
  },
  "portals": [
    "ATSD"
  ],
  "all-entities-read": false,
  "all-entities-write": true,
  "all-portals-permission": false
}
```

* API user:

```json
{
  "roles": ["ROLE_API_DATA_WRITE", "ROLE_API_DATA_READ", "ROLE_API_META_WRITE", "ROLE_API_META_READ"],
  "user-groups": ["data-meta-all-entity-write-collectors", "docker-entities-read"],
  "entity-groups": {
    "docker-containers": "READ",
    "docker-images": "READ",
    "docker-networks": "READ",
    "docker-volumes": "READ"
  },
  "all-entities-read": false,
  "all-entities-write": true,
  "all-portals-permission": false,
  "portals": []
}
```

* Admin user:

```json
{
  "roles": ["ROLE_ADMIN"] ,
  "user-groups":[],
  "entity-groups":{},
  "all-entities-read":true,
  "all-entities-write":true,
  "all-portals-permission":true,
  "portals":[]
}
```