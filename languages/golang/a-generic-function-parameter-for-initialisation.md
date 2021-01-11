# A generic function parameter for initialisation

{% embed url="https://github.com/victorspringer/http-cache" %}

Look for NewClient implementation

```text
func NewClient(opts ...ClientOption) (*Client, error) {
	c := &Client{}

	for _, opt := range opts {
		if err := opt(c); err != nil {
			return nil, err
		}
	}
}
```

This client can be initialised with multiple options/user preferences.

```text
// ClientOption is used to set Client settings.
type ClientOption func(c *Client) error
```

```text
// ClientWithAdapter sets the adapter type for the HTTP cache
// middleware client.
func ClientWithAdapter(a Adapter) ClientOption {
	return func(c *Client) error {
		c.adapter = a
		return nil
	}
}

// ClientWithTTL sets how long each response is going to be cached.
func ClientWithTTL(ttl time.Duration) ClientOption {
	return func(c *Client) error {
		if int64(ttl) < 1 {
			return fmt.Errorf("cache client ttl %v is invalid", ttl)
		}

		c.ttl = ttl

		return nil
	}
}
```

Every option is of type function which takes a client as input. When these options \(functions\) are passed by the user, we just simply call those function by passing in the new client as a parameter. The function/options takes care of modifying the client accordingly

