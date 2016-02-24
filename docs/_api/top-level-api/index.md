---
layout: page
title: Top-Level API
id: top-level
section: Top-Level API
---

<h2 id="registration">Registration</h2>

<h3 id="register">Marty.register(class, [id])</h3>

Register a Marty class into the [registry](#registry). Instantiates an instance with the [dispatcher]({% url /api/dispatcher/index.html#default %}) and returns it. Optionally takes a store Id as the second argument. Defaults to the store name.

{% highlight js %}
class UserStore extends Marty.Store {
  ...
}

module.exports = Marty.register(UserStore, 'users');
{% endhighlight %}

<h3 id="createStore">Marty.createStore(options)</h3>

Register a [store]({% url /api/stores/index.html %}) defined with the ES5 syntact into the [registry](#registry). Instantiates an instance with the [dispatcher]({% url /api/dispatcher/index.html#default %}) and returns it.

<h3 id="createQueries">Marty.createQueries(options)</h3>

Register [queries]({% url /api/queries/index.html %}) defined with the ES5 syntact into the [registry](#registry). Instantiates an instance with the [dispatcher]({% url /api/dispatcher/index.html#default %}) and returns it.

<h3 id="createStateSource">Marty.createStateSource(options)</h3>

Register a [state source]({% url /api/state-sources/index.html %}) defined with the ES5 syntact into the [registry](#registry). Instantiates an instance with the [dispatcher]({% url /api/dispatcher/index.html#default %}) and returns it.

<h3 id="createActionCreators">Marty.createActionCreators(options)</h3>

Register [action creators]({% url /api/action-creators/index.html %}) defined with the ES5 syntact into the [registry](#registry). Instantiates an instance with the [dispatcher]({% url /api/dispatcher/index.html#default %}) and returns it.

<h3 id="createStateMixin">Marty.createStateMixin(options)</h3>

Creates a [state mixin]({% url /guides/state-mixin/index.html %}) and returns it.

<h3 id="createConstants">Marty.createConstants(options)</h3>

Creates [constants]({% url /guides/constants/index.html %}) and returns them.

<h3 id="createContainer">Marty.createContainer(InnerComponent, options)</h3>

Wraps the component with a [container component]({% url /guides/containers/index.html %}) that is responsible for fetching state from stores and passing it to the inner component.

<h3 id="registry">Marty.registry</h3>

Returns the current instance of the [registry]({% url /api/registry/index.html %}).

<h2 id="state">State</h2>
<h3 id="replaceState">Marty.replaceState(stores, [context])</h3>

Replaces ([Store#replaceState]({% url /api/stores/index.html#replaceState %})) the state of all stores with the values passed in. If a [context](#createContext) is passed in then we will replace the context's stores state. The key within the ``stores`` object literal must be the Id of the store.

{% highlight js %}
Marty.replaceState({
  UserStore: {
    state: {
      [123]: { id: 123, name: 'Foo' }
    }
  }
});

UserStore.getUser(123) // { id: 123, name: 'Foo' }
{% endhighlight %}

<h3 id="clearState">Marty.clearState([context])</h3>
Calls [Store#clear]({% url /api/stores/index.html#clear %}) on all registered stores. If a [context](#createContext) is passed in then we will call  [Store#clear]({% url /api/stores/index.html#clear %})) on all stores within the context.

<h3 id="dehydrate">Marty.dehydrate([context])</h3>

Calls [Store#dehydrate]({% url /api/stores/index.html#dehydrate %}) if present or [Store#getState]({% url /api/stores/index.html#getState %}) on all registered stores. Returning all states as a serializable object literal where they key is the Id of the store. If a [context](#createContext) is passed in then we will dehyrdate all stores within the context.

<h3 id="rehydrate">Marty.rehydrate([states], [context])</h3>

Given some dehyrdated state, it will call [Store#rehydrate]({% url /api/stores/index.html#rehydrate %}) if present or [Store#replaceState]({% url /api/stores/index.html#replaceState %}) on all registered stores passing in the dehyrdated state. The key of the states must match the Id of the store. If you don't pass in states then it will look at the ``window.__marty.state``. If a [context](#createContext) is passed in then we will rehyrdate all stores within the context.

<h3 id="createContext">Marty.createContext([values])</h3>

Creates a [context]({% url /api/context/index.html %}) which contains a dispatcher and instances of all types currently registered within [Marty.registry](#registry). Optionally will be extended with ``values`` object.

<h3 id="renderToString">Marty.renderToString(options)</h3>

[Renders](http://facebook.github.io/react/docs/top-level-api.html#react.rendertostring) the given component type with the given props to string, waits for all fetches to complete and then re-renders component. Returns a promise which resolves once component is re-rendered. Result of render is an object containing the rendered string and an object detailing what fetches occurred. ``timeout`` allows you to configure how long to wait for a fetch to finish before re-rendering the component (Default **1000ms**). It uses React contexts to pass the Marty context to child components (context key is ``marty``).

{% highlight js %}
var options = {
  type: Foo,
  context: context,
  props: { bar: 'bar' },
  timeout: 1000
};

var Foo = React.createClass({
  contextTypes {
    marty: React.PropTypes.object
  },
  render: function () {
    var store = this.context.marty.getStore('foo');

    return <div />
  }
});

Marty.renderToString(options).then(function (res) {
  console.log('Rendered html', res.html);
  console.log('Diagnostics', res.diagnostics);
});
{% endhighlight %}

<h2 id="environment">Environment</h2>
<h3 id="isServer">Marty.isServer</h3>

True if the current process is being executed within node.js or io.js.

<h3 id="isBrowser">Marty.isBrowser</h3>

True if the current process is being executed within a browser.

<h2 id="general">General</h2>
<h3 id="dispatcher">Marty.dispatcher</h3>

Returns the current instance of the [dispatcher]({% url /api/dispatcher/index.html %}).

<h3 id="warnings">Marty.warnings</h3>

Configurable list of warnings that Marty emits. Setting the key to false will stop the warning from happening. We will warn you when a feature is being depreciated so disabling warnings can make upgrading difficult in the future.

{% highlight js %}
Marty.warnings.invokeConstant = false;
{% endhighlight %}

<h4 id="withoutWarning">Marty.warnings.without(warningsToDisable*, callback, [context])</h4>

Disables a warning for the duration of the callback.

{% highlight js %}
Marty.warnings.without(['reservedFunction', 'superNotCalledWithOptions'], function () {
  // do something evil
});
{% endhighlight %}

<h3 id="createInstance">Marty.createInstance()</h3>

Creates a new instance of Marty. Useful for testing.

<h3 id="dispose">Marty.dispose()</h3>

Disposes of all state and event listeners for all known instances.

<h3 id="version">Marty.version</h3>

The current version of Marty.
