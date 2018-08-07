# Loopback v3 utils

Utility classes with decorators to help reduce boilerplate code and add types to loopback v3 model apis.

- Model Api classes
- Separate Model Api / Remote methods into definition and implimentation classes
- Clearly define Remote Methods and Operation Hooks with method deorators

## LoopbackModelApi

A base class for creating classes of remote methods.

    export class JediModelApi extends LoopbackModelApi {
        constructor(private jediApi: LoopbackModelApiWithApp<JediApi>) {
            super(jediApi);
        }
    }

### @RemoteMethod()

A decorator for creating remote methods in your LoopbackModelApi class

      @RemoteMethod()
      public searchFeelings: RemoteMethodDefinition = {
        description: 'Search your feelings',
        path: ':id/search-your-feelings/',
        verb: 'GET',
        accepts: [{ arg: 'id', type: 'string', required: true }],
        returns: { arg: 'knownItToBeTrue', type: 'boolean', root: true },
        method: (feelings: boolean) => feelings === true;
      };

### @OperationHook(operationHookName: string)

A decorator that subscribes to [Loopback operation hooks](https://loopback.io/doc/en/lb2/Operation-hooks.html) for this Model Api.

@OperationHook('before delete')
private async afterSave(context: any) {
console.log(`Execute Order 66 on Master ${context.instance.lastName}`);
}

## LoopbackModelApiImplementation

You can separate the Remote Method definition and Remote Method implimentation into separate files (here is how: [including your implimentation class as a mixin](https://www.google.com)) and extend the LoopbackModelApiImplementation class.

**Note:** When using the @RemoteMethodImplimentation() decorator omit the method propety of the @RemoteMethod() decorator.

    export class JediModelApiImplimentation extends LoopbackModelApiImplementation {
        constructor(private jediApi: LoopbackModelApiWithApp<JediApi>) {
            super(jediApi);
        }
    }

### @RemoteMethodImplimentation()

A decorator for creating remote method implimentations inside your LoopbackModelApiImplementation class.

    @RemoteMethodImplimentation()
    private async searchFeelings(feelings: boolean): Promise<boolean> {
        return feelings === true;
    }
