For https://github.com/flutter/flutter/issues/164670

# Instruction:

1. Drag `MyShareExtensionDismissControlRecognizer.h` and `MyShareExtensionDismissControlRecognizer.m` files into your extension target. 

2. Add the recognizer to `ShareViewController`: 

```
@implementation ShareViewController

- (void)viewDidLoad {
  [super viewDidLoad];
  
  UIGestureRecognizer *recognizer = [[MyShareExtensionDismissControlRecognizer alloc] initWithDismissStrategy:MyShareExtensionSwipeDismissStrategyTopRegion];
  [self.view addGestureRecognizer:recognizer];
```

## Dimiss strategy

- With `MyShareExtensionSwipeDismissStrategyTopRegion`, you can drag the top region of the sheet to dismiss it. 
- With `MyShareExtensionSwipeDismissStrategyProhibited`, you can't dismiss the via gesture. Instead, you have to use a button. 

## Dismiss button

You can either add the button on native side or on flutter side. 

1. To add the dismiss button on native side, you can simply use a navigation bar: 

```
- (void)viewDidLoad {
  [super viewDidLoad];

  FlutterViewController *vc = [[FlutterViewController alloc] initWithProject:nil nibName:nil bundle:nil];
  UINavigationController *nav = [[UINavigationController alloc] initWithRootViewController:vc];
  [self addChildViewController:nav];
  [self.view addSubview:nav.view];
  nav.view.frame = self.view.bounds;
  
  vc.navigationItem.rightBarButtonItem = [[UIBarButtonItem alloc] initWithTitle:@"Save" style:UIBarButtonItemStyleDone target:self action:@selector(saveButtonClicked)];
}
- (void)saveButtonClicked {
  NSLog(@"save button clicked");
  
  [self.extensionContext completeRequestReturningItems:nil completionHandler:^(BOOL expired) {
    NSLog(@"Dismissed");
  }];
}
```

2. To add the dismiss button on flutter side, you can use a method channel, and call the `completeRequestReturningItems` API. 

## Demo

### Top Region

<video width="320" height="240" controls>
 <source src="https://github.com/user-attachments/assets/2187d213-e506-474b-9418-10b85c3fb864" type="video/mp4">
</video>


![](./top_region.MOV)

<video width="320" height="240" controls>
 <source src="./top_region.MOV" type="video/mp4">
</video>

### Dismiss button


<video width="320" height="240" controls>
 <source src="./dismiss_button.MOV" type="video/mp4">
</video>

### Top Region + Dismiss Button

<video width="320" height="240" controls>
 <source src="./both.MOV" type="video/mp4">
</video>



