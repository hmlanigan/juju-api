# Juju REST API

> This is very much speculative, and a work in progress!

This repository serves to assist with a WIP design for a new RESTful Juju client API.

The existing API surface for Juju is rather large, and the aim here is to design a new client API,
and not intervene with the controller <-> controller APIs or controller <-> agent APIs.

## Getting Started

Assuming you have a working NodeJS and Python environment set up:

```shell
# Clone the repo
git clone https://github.com/canonical/juju-api
cd juju-api

# Serve the site(s) using Docker
make serve
```

You can now see the newly designed spec at http://localhost:8080/ and the generated spec at
http://localhost:8080/generated

## Updating the schemas/sites

```shell
# Generate a naive OpenAPI spec from the Juju client facade schema
python3 tools/convert-juju-facade -i schemas/client-schemas.json -o schemas/generated.yaml
```

## Generating Juju schema files

You can find the _client facades_ in [./schemas/client-schemas.json](./schemas/client-schemas.json).

These were generated like so:

```shell
git clone https://github.com/juju/juju
cd juju/generate/schemagen

# This will output the file client-facades.json
go run schemagen.go -admin-facades -facade-group client ./schemas/client-schemas.json
```

### Progress

This table tracks progress through the redesign of the Juju client API:

| Facade               | Facade Call                     | API Path                                                        |
| :------------------- | :------------------------------ | :-------------------------------------------------------------- |
| Action               | Actions                         |                                                                 |
| Action               | ApplicationsCharmsActions       |                                                                 |
| Action               | Cancel                          |                                                                 |
| Action               | EnqueueOperation                |                                                                 |
| Action               | ListOperations                  |                                                                 |
| Action               | Operations                      |                                                                 |
| Action               | Run                             |                                                                 |
| Action               | RunOnAllMachines                |                                                                 |
| Action               | WatchActionsProgress            |                                                                 |
| Admin                | Login                           |                                                                 |
| Admin                | RedirectInfo                    |                                                                 |
| AllModelWatcher      | Next                            |                                                                 |
| AllModelWatcher      | Stop                            |                                                                 |
| AllWatcher           | Next                            |                                                                 |
| AllWatcher           | Stop                            |                                                                 |
| Annotations          | Get                             |                                                                 |
| Annotations          | Set                             |                                                                 |
| Application          | AddRelation                     | `POST /models/{namespace}/{model}/integrations`                 |
| Application          | AddUnits                        | `PATCH /models/{namespace}/{model}/apps/{app}/scale`            |
| Application          | ApplicationsInfo                | `GET /models/{namespace}/{model}/apps/{app}`                    |
| Application          | CharmConfig                     | `GET /models/{namespace}/{model}/apps/{app}`                    |
| Application          | CharmRelations                  |                                                                 |
| Application          | Consume                         |                                                                 |
| Application          | Deploy                          | `POST /models/{namespace}/{model}/apps`                         |
| Application          | DeployFromRepository            | `POST /models/{namespace}/{model}/apps`                         |
| Application          | DestroyApplication              | `DELETE /models/{namespace}/{model}/apps/{app}`                 |
| Application          | DestroyConsumedApplications     |                                                                 |
| Application          | DestroyRelation                 | `DELETE /models/{namespace}/{model}/integrations/{integration}` |
| Application          | DestroyUnit                     | `PATCH /models/{namespace}/{model}/apps/{app}/scale`            |
| Application          | Expose                          | `PATCH /models/{namespace}/{model}/apps/{app}`                  |
| Application          | Get                             | `GET /models/{namespace}/{model}/apps/{app}`                    |
| Application          | GetConfig                       | `GET /models/{namespace}/{model}/apps/{app}`                    |
| Application          | GetConstraints                  | `GET /models/{namespace}/{model}/apps/{app}`                    |
| Application          | Leader                          |                                                                 |
| Application          | MergeBindings                   |                                                                 |
| Application          | ResolveUnitErrors               |                                                                 |
| Application          | ScaleApplications               | `PATCH /models/{namespace}/{model}/apps/{app}/scale`            |
| Application          | SetCharm                        | `PATCH /models/{namespace}/{model}/apps/{app}/refresh`          |
| Application          | SetConfigs                      | `PATCH /models/{namespace}/{model}/apps/{app}`                  |
| Application          | SetConstraints                  | `PATCH /models/{namespace}/{model}/apps/{app}`                  |
| Application          | SetMetricCredentials            |                                                                 |
| Application          | SetRelationsSuspended           |                                                                 |
| Application          | Unexpose                        | `PATCH /models/{namespace}/{model}/apps/{app}`                  |
| Application          | UnitsInfo                       |                                                                 |
| Application          | UnsetApplicationsConfig         | `PATCH /models/{namespace}/{model}/apps/{app}`                  |
| Application          | UpdateApplicationBase           | `PATCH /models/{namespace}/{model}/apps/{app}:refresh`          |
| ApplicationOffers    | ApplicationOffers               |                                                                 |
| ApplicationOffers    | DestroyOffers                   |                                                                 |
| ApplicationOffers    | FindApplicationOffers           |                                                                 |
| ApplicationOffers    | GetConsumeDetails               |                                                                 |
| ApplicationOffers    | ListApplicationOffers           |                                                                 |
| ApplicationOffers    | ModifyOfferAccess               |                                                                 |
| ApplicationOffers    | Offer                           |                                                                 |
| ApplicationOffers    | RemoteApplicationInfo           |                                                                 |
| Backup               | Create                          |                                                                 |
| Block                | List                            |                                                                 |
| Block                | SwitchBlockOff                  |                                                                 |
| Block                | SwitchBlockOn                   |                                                                 |
| Bundle               | ExportBundle                    |                                                                 |
| Bundle               | GetChanges                      |                                                                 |
| Bundle               | GetChangesMapArgs               |                                                                 |
| Charms               | AddCharm                        |                                                                 |
| Charms               | CheckCharmPlacement             |                                                                 |
| Charms               | GetDownloadInfos                |                                                                 |
| Charms               | IsMetered                       |                                                                 |
| Charms               | List                            |                                                                 |
| Charms               | ListCharmResources              |                                                                 |
| Charms               | ResolveCharms                   |                                                                 |
| Client               | FindTools                       |                                                                 |
| Client               | FullStatus                      |                                                                 |
| Client               | StatusHistory                   |                                                                 |
| Client               | WatchAll                        |                                                                 |
| Cloud                | AddCloud                        | `POST /clouds`                                                  |
| Cloud                | AddCredentials                  |                                                                 |
| Cloud                | CheckCredentialsModels          |                                                                 |
| Cloud                | Cloud                           | `GET /clouds/{cloud}`                                           |
| Cloud                | CloudInfo                       | `GET /clouds/{cloud}`                                           |
| Cloud                | Clouds                          | `GET /clouds?show-supported=true`                               |
| Cloud                | Credential                      |                                                                 |
| Cloud                | CredentialContents              |                                                                 |
| Cloud                | InstanceTypes                   |                                                                 |
| Cloud                | ListCloudInfo                   | `GET /clouds`                                                   |
| Cloud                | ModifyCloudAccess               | `PATCH /clouds/{cloud}`                                         |
| Cloud                | RemoveClouds                    | `DELETE /clouds/{cloud}`                                        |
| Cloud                | RevokeCredentialsCheckModels    |                                                                 |
| Cloud                | UpdateCloud                     |                                                                 |
| Cloud                | UpdateCredentialsCheckModels    |                                                                 |
| Cloud                | UserCredentials                 |                                                                 |
| Controller           | AllModels                       |                                                                 |
| Controller           | CloudSpec                       |                                                                 |
| Controller           | ConfigSet                       |                                                                 |
| Controller           | ControllerAPIInfoForModels      |                                                                 |
| Controller           | ControllerConfig                |                                                                 |
| Controller           | ControllerVersion               |                                                                 |
| Controller           | DashboardConnectionInfo         |                                                                 |
| Controller           | DestroyController               |                                                                 |
| Controller           | GetCloudSpec                    |                                                                 |
| Controller           | GetControllerAccess             |                                                                 |
| Controller           | HostedModelConfigs              |                                                                 |
| Controller           | IdentityProviderURL             |                                                                 |
| Controller           | InitiateMigration               |                                                                 |
| Controller           | ListBlockedModels               |                                                                 |
| Controller           | ModelConfig                     |                                                                 |
| Controller           | ModelStatus                     |                                                                 |
| Controller           | ModifyControllerAccess          |                                                                 |
| Controller           | MongoVersion                    |                                                                 |
| Controller           | RemoveBlocks                    |                                                                 |
| Controller           | WatchAllModelSummaries          |                                                                 |
| Controller           | WatchAllModels                  |                                                                 |
| Controller           | WatchCloudSpecsChanges          |                                                                 |
| Controller           | WatchModelSummaries             |                                                                 |
| CredentialManager    | InvalidateModelCredential       |                                                                 |
| FirewallRules        | ListFirewallRules               |                                                                 |
| FirewallRules        | SetFirewallRules                |                                                                 |
| HighAvailability     | EnableHA                        |                                                                 |
| ImageMetaDataManager | Delete                          |                                                                 |
| ImageMetaDataManager | List                            |                                                                 |
| ImageMetaDataManager | Save                            |                                                                 |
| KeyManager           | AddKeys                         |                                                                 |
| KeyManager           | DeleteKeys                      |                                                                 |
| KeyManager           | ImportKeys                      |                                                                 |
| KeyManager           | ListKeys                        |                                                                 |
| MachineManager       | AddMachines                     | `POST /models/{namespace}/{model}/machines`                     |
| MachineManager       | DestroyMachineWithParams        | `DELETE /models/{namespace}/{model}/machines/{machine-id}`      |
| MachineManager       | GetUpgradeSeriesMessages        |                                                                 |
| MachineManager       | InstanceTypes                   |                                                                 |
| MachineManager       | ProvisioningScript              |                                                                 |
| MachineManager       | RetryProvisioning               |                                                                 |
| MachineManager       | UpgradeSeriesComplete           |                                                                 |
| MachineManager       | UpgradeSeriesPrepare            |                                                                 |
| MachineManager       | UpgradeSeriesValidate           |                                                                 |
| MachineManager       | WatchUpgradeSeriesNotifications |                                                                 |
| MetricsDebug         | GetMetrics                      |                                                                 |
| MetricsDebug         | SetMeterStatus                  |                                                                 |
| ModelConfig          | GetModelConstraints             | `GET /models/{namespace}/{model}`                               |
| ModelConfig          | ModelGet                        | `GET /models/{namespace}/{model}`                               |
| ModelConfig          | ModelSet                        | `PATCH /models/{namespace}/{model}`                             |
| ModelConfig          | ModelUnset                      | `PATCH /models/{namespace}/{model}`                             |
| ModelConfig          | SLALevel                        | N/A                                                             |
| ModelConfig          | Sequences                       |                                                                 |
| ModelConfig          | SetModelConstraints             | `PATCH /models/{namespace}/{model}`                             |
| ModelConfig          | SetSLALevel                     | N/A                                                             |
| ModelGeneration      | AbortBranch                     |                                                                 |
| ModelGeneration      | AddBranch                       |                                                                 |
| ModelGeneration      | BranchInfo                      |                                                                 |
| ModelGeneration      | CommitBranch                    |                                                                 |
| ModelGeneration      | HasActiveBranch                 |                                                                 |
| ModelGeneration      | ListCommits                     |                                                                 |
| ModelGeneration      | ShowCommit                      |                                                                 |
| ModelGeneration      | TrackBranch                     |                                                                 |
| ModelManager         | ChangeModelCredential           | `PATCH /models/{namespace}/{model}`                             |
| ModelManager         | CreateModel                     | `POST /models`                                                  |
| ModelManager         | DestroyModels                   | `DELETE /models/{namespace}/{model}`                            |
| ModelManager         | DumpModels                      | `GET /models?detailed=true&all=true`                            |
| ModelManager         | DumpModelsDB                    | N/A                                                             |
| ModelManager         | ListModelSummaries              | `GET /models?detailed=true`                                     |
| ModelManager         | ListModels                      | `GET /models`                                                   |
| ModelManager         | ModelDefaultsForClouds          | `GET /model-defaults`                                           |
| ModelManager         | ModelInfo                       | `GET /models/{namespace}/{model}`                               |
| ModelManager         | ModelStatus                     | `GET /models/{namespace}/{model}/status`                        |
| ModelManager         | ModifyModelAccess               | `PATCH /models/{namespace}/{model}`                             |
| ModelManager         | SetModelDefaults                | `PATCH /model-defaults`                                         |
| ModelManager         | UnsetModelDefaults              | `PATCH /model-defaults`                                         |
| ModelUpgrader        | AbortModelUpgrade               |                                                                 |
| ModelUpgrader        | UpgradeModel                    |                                                                 |
| Resources            | AddPendingResources             |                                                                 |
| Resources            | Attach                          |                                                                 |
| Resources            | ListResources                   |                                                                 |
| SSHClient            | AllAddresses                    |                                                                 |
| SSHClient            | ModelCredentialForSSH           |                                                                 |
| SSHClient            | PrivateAddress                  |                                                                 |
| SSHClient            | Proxy                           |                                                                 |
| SSHClient            | PublicAddress                   |                                                                 |
| SSHClient            | PublicKeys                      |                                                                 |
| SecretBackends       | AddSecretBackends               | `POST /secret-backends`                                         |
| SecretBackends       | ListSecretBackends              | `GET /secret-backends`                                          |
| SecretBackends       | RemoveSecretBackends            | `DELETE /secret-backends/{secret-backend}`                      |
| SecretBackends       | UpdateSecretBackends            | `PATCH /secret-backends/{secret-backend}`                       |
| Secrets              | ListSecrets                     | `GET /secrets`                                                  |
| Spaces               | CreateSpaces                    | `POST /spaces`                                                  |
| Spaces               | ListSpaces                      | `GET /spaces`                                                   |
| Spaces               | MoveSubnets                     | `PATCH /spaces/{space}`                                         |
| Spaces               | ReloadSpaces                    | `GET /spaces?reload=true`                                       |
| Spaces               | RemoveSpace                     | `DELETE /spaces/{space}`                                        |
| Spaces               | RenameSpace                     | `PATCH /spaces/{space}`                                         |
| Spaces               | ShowSpace                       | `GET /spaces/{space}`                                           |
| Storage              | AddToUnit                       |                                                                 |
| Storage              | Attach                          |                                                                 |
| Storage              | CreatePool                      |                                                                 |
| Storage              | DetachStorage                   |                                                                 |
| Storage              | Import                          |                                                                 |
| Storage              | ListFilesystems                 |                                                                 |
| Storage              | ListPools                       |                                                                 |
| Storage              | ListStorageDetails              |                                                                 |
| Storage              | ListVolumes                     |                                                                 |
| Storage              | Remove                          |                                                                 |
| Storage              | RemovePool                      |                                                                 |
| Storage              | StorageDetails                  |                                                                 |
| Storage              | UpdatePool                      |                                                                 |
| Subnets              | AllZones                        | `GET /subnets/zones`                                            |
| Subnets              | ListSubnets                     | `GET /subnets`                                                  |
| Subnets              | SubnetsByCIDR                   | `GET /subnets?cidrs=172.30.34.1/24`                             |
| UserManager          | AddUser                         | `POST /users`                                                   |
| UserManager          | DisableUser                     | `PATCH /users/{user}`                                           |
| UserManager          | EnableUser                      | `PATCH /users/{user}`                                           |
| UserManager          | ModelUserInfo                   | `GET /models?user={user}`                                       |
| UserManager          | RemoveUser                      | `DELETE /users/{user}`                                          |
| UserManager          | ResetPassword                   | `PATCH /users/{user}`                                           |
| UserManager          | SetPassword                     | `PATCH /users/{user}`                                           |
| UserManager          | UserInfo                        | `GET /users/{user}`                                             |
