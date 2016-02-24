---
layout: page
title: Containers
id: container
section: Containers
---

We found that there was a lot of boilerplate code in React components for listening to [stores]({% url /guides/stores/index.html %}) and getting state from them. Containers provide a simpler way for your components to interact with stores. They do this by wrapping your component in another component (Called a **container component**) which is responsible for fetching state from the stores which it then passes to the inner component via props.

{% sample %}
classic
=======
var User = React.createClass({
  render: function() {
    return <div className="User">{this.props.user}</div>;
  }
});

module.exports = Marty.createContainer(User, {
  listenTo: UserStore,
  fetch: {
    user: function() {
      return UserStore.for(this).getUser(this.props.id);
    }
  },
  failed(errors) {
    return <div className="User User-failedToLoad">{errors}</div>;
  },
  pending() {
    return this.done({
      user: {}
    });
  }
});

es6
===
class User extends React.Component {
  render() {
    return <div className="User">{this.props.user}</div>;
  }
}

module.exports = Marty.createContainer(User, {
  listenTo: UserStore,
  fetch: {
    user() {
      return UserStore.for(this).getUser(this.props.id);
    }
  },
  failed(errors) {
    return <div className="User User-failedToLoad">{errors}</div>;
  },
  pending() {
    return this.done({
      user: {}
    });
  }
});
{% endsample %}

To create a container, pass your component to [``Marty.createContainer``]({% url /api/top-level-api/index.html#createContainer %}) with an object hash that contains the container configuration. [``Marty.createContainer``]({% url /api/top-level-api/index.html#createContainer %}) will return a new component which knows how to fetch state from stores as well as rendering it. 

The most important configuration option is [``fetch``]({% url /api/containers/index.html#fetch %}). [``fetch``]({% url /api/containers/index.html#fetch %}) is an object where the values are functions which are invoked and the result is passed to the inner component as a prop. The prop key is determined by the key. The container component will do this when first created and any time any store you specify in [``listenTo``]({% url /api/containers/index.html#listenTo %}) changes.

If you're using the [fetch API]({% url /api/stores/index.html#fetch %}) then containers provide an easy way of dealing with the different states a fetch can be in. If any of your fetches are pending then the container will render whatever you return from the [``pending`` handler]({% url /api/containers/index.html#pending %}). The same will happen if any of the fetches have failed however the container will pass in an object containing all the errors to the [``failed`` handler]({% url /api/containers/index.html#failed %}).

##Further reading

* [Separating components and containers in different files]({% url /guides/containers/separating-components-and-containers-in-different-files.html %}).
* [Mixins Are Dead. Long Live Composition](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750)
* [Building The Facebook News Feed With Relay](http://facebook.github.io/react/blog/2015/03/19/building-the-facebook-news-feed-with-relay.html)

