---
layout: post
title:  “努力尝试新的事物”
date:   2016-03-16 00:55:48
categories: jekyll update
---


发现一个问题,现在的时间是2016年03月16日10:58:16,当我在jekyll上面用现在的时间的时候,文章不能显示.
的确是有点诡异.
我估摸着是和东八区的时间有关系,往前调了10个小时就又正常显示的了.是不是也和

// 识别剪切板的内容
- (void)tellPasteBoard
{
    // 识别剪切板的链接.
    UIPasteboard *pasteboard = [UIPasteboard generalPasteboard];
    
    if ([pasteboard.string isUrl]) {
        NSURL *url = [NSURL URLWithString:pasteboard.string];
        if ([url.host isEqualToString:@"homeusa.cn"]) {
            // 抽离query拼成字典
            NSDictionary *data = [url.query dictionaryFromQueryComponents];
            
            [pasteboard setValue:@"" forPasteboardType:UIPasteboardNameGeneral]; // 清除响应过的剪切板.只在打开过后响应一次.
            [self pushData:data];
        }
    } else if ([pasteboard.string hasPrefix:@"#悦房优惠#"]) {
        NSString *temp = [pasteboard.string substringFromIndex:pasteboard.string.length - 8];
        DiscountViewController *disVC = [[DiscountViewController alloc] init];
        disVC.getCode = YES;
        disVC.code = temp;
        [pasteboard setValue:@"" forPasteboardType:UIPasteboardNameGeneral]; // 清除响应过的剪切板.
        
        MMDrawerController *drawNC = (MMDrawerController *)self.window.rootViewController;
        [drawNC closeDrawerAnimated:YES completion:nil];
        UINavigationController *navigation = (UINavigationController *)drawNC.centerViewController;
        UIViewController *nowVC = [navigation.viewControllers lastObject];
        [nowVC.navigationController pushViewController:disVC animated:YES];
    }
}