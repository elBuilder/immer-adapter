# immer-adapter

Before

```ts
import { State, StateContext } from '@ngxs/store';
import { Receiver, EmitterAction } from '@ngxs-labs/emitter';

@State<AnimalStateModel>({
  name: 'zoo',
  defaults: {
    zebra: { food: [], name: 'zebra' },
    panda: { food: [], name: 'panda' }
  }
})
export class AnimalState {

  @Receiver()
  public feedZebra(ctx: StateContext<AnimalStateModel>, action: EmitterAction<FeedZebraModel>) {
    const state = ctx.getState();
    ctx.setState({
      ...state,
      zebra: {
         ...state.zebra,
         food: [ ...state.zebra.food,  action.zebraToFeed]
      }
    });
  }
  
}
```

After

```ts
import { State, StateContext } from '@ngxs/store';
import { Reciver, EmitterAction } from '@ngxs-labs/emitter';
import { produce } from '@ngxs-labs/immer-adapter';

@State<AnimalStateModel>({
  name: 'zoo',
  defaults: {
    zebra: { food: [], name: 'zebra' },
    panda: { food: [], name: 'panda' }
  }
})
export class AnimalState {
  
  @Receiver()
  public feedZebra(ctx: StateContext<AnimalStateModel>, action: EmitterAction<FeedZebraModel>) {
    produce(ctx, (draft: AnimalStateModel) => draft.zebra.food.push(action.zebraToFeed));
  }
  
}
```
