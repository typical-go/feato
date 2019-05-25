# FEATO

**Fea**-ture **To**-ggle Library for Go.

- Global Access Instance (Singleton)
- Dynamic Handler (Realtime update)
- Avoid condition (No `if` statement)

## Example

```go
var (
	// feature with specific ID
	featureHelloWorld = feato.NewFeature("hello-world")

	// unique feature
	feature1 = feato.NewUniqueFeature()

	// feature is enable by default
	feature2 = feato.NewUniqueFeature().EnableByDefault()

	// feature have specific index toggle to route to
	feature3 = feato.NewUniqueFeature().SetDefaultIndexToggle(feato.DisableIndexToggle)
)

func init() {
	// Set ToggleRouter to define which is enable
	feato.SetToggleRouter(feato.NewSimpleToggleRouter().
		SetIndexToggle("hello-world", feato.EnableIndexToggle).
		Enable(feature1).
		Disable(feature2).
		SetFeature(feature3, feato.EnableIndexToggle))
}

func main() {
	feato.Run(featureHelloWorld, func() (err error) {
		fmt.Println("Hello World")
		return
	})
	feato.Run(feature1, feature1Func, feature1AlternativeFunc)

	// ok is identify if function run
	ok, _ := feato.Run(feature2, feature2Func)
	if !ok {
		log.Println("Feature2 is not running")
	}

	log.Println(runFeature3())
}

func runFeature3() error {
	_, err := feato.Run(feature3,
		func() error {
			// By pass error to caller
			return fmt.Errorf("something error on feature 3")
		},
		func() error {
			return fmt.Errorf("feature 3 alternateve also got error")
		})

	return err
}
```

## What's next?

- [ ] Nested feature
- [ ] `tags` or `namespace` to categorize features and enabling/disabling by it 
- [ ] Get data from Environment Variable
- [ ] Get data from embedded database - [bbolt](https://github.com/etcd-io/bbolt) as feature storage 
- [ ] Get data from external service integration e.g [flagr](https://github.com/checkr/flagr) as feature storage
- [ ] AB Testing Feature Type
- [ ] User Target Feature Type


## References

- <https://medium.com/@dehora/feature-flags-smaller-better-faster-software-development-f2eab58df0f9>
- <https://martinfowler.com/articles/feature-toggles.html>
