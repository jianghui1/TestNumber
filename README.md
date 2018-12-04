##### 做过电商项目的同学应该都能体会到价格计算的心塞，四舍五入、向下取整、向上取整，保留1位、保留2位、计算结果小数位有0抹去、计算偏差等等，今天就来总结下这些个东西的应对之法。

先说前提，主要针对正数，负数可能不适用。

再说结论：
1. `.f/.1f/.2f` 等等，并不是四舍五入，而是五舍六入。
2. 向上取整、向下取整有专门的方法。
3. 保留几位，使用`NSNumberFormatter`，结果小数部分多余的零会自动抹去。
4. 计算偏差，末位为零或者计算中出现了`.19999`(其实是`.2`)这种，可以通过加上 `0.000001` 来解决。

## 浮点数的四舍五入，向下取整、向上取整
代码如下：

    #pragma mark - Float 四舍五入、向上取整、向下取整
    - (void)testFloat
    {
        // 并不是四舍五入的，而是五舍六入
        CGFloat f0 = 1.1;
        CGFloat f1 = 1.11;
        CGFloat f2 = 1.12;
        CGFloat f3 = 1.13;
        CGFloat f4 = 1.14;
        CGFloat f5 = 1.15;
        CGFloat f6 = 1.16;
        CGFloat f7 = 1.17;
        CGFloat f8 = 1.18;
        CGFloat f9 = 1.19;
        NSLog(@"testFloat -- %.1f", f0);
        NSLog(@"testFloat -- %.1f", f1);
        NSLog(@"testFloat -- %.1f", f2);
        NSLog(@"testFloat -- %.1f", f3);
        NSLog(@"testFloat -- %.1f", f4);
        NSLog(@"testFloat -- %.1f", f5);
        NSLog(@"testFloat -- %.1f", f6);
        NSLog(@"testFloat -- %.1f", f7);
        NSLog(@"testFloat -- %.1f", f8);
        NSLog(@"testFloat -- %.1f", f9);
        
        // 打印日志
        /*
         2018-12-03 11:40:49.112866+0800 TestNumber[17083:101991] testFloat -- 1.1
         2018-12-03 11:40:49.113083+0800 TestNumber[17083:101991] testFloat -- 1.1
         2018-12-03 11:40:49.113183+0800 TestNumber[17083:101991] testFloat -- 1.1
         2018-12-03 11:40:49.113271+0800 TestNumber[17083:101991] testFloat -- 1.1
         2018-12-03 11:40:49.113353+0800 TestNumber[17083:101991] testFloat -- 1.1
         2018-12-03 11:40:49.113431+0800 TestNumber[17083:101991] testFloat -- 1.1
         2018-12-03 11:40:49.113517+0800 TestNumber[17083:101991] testFloat -- 1.2
         2018-12-03 11:40:49.113839+0800 TestNumber[17083:101991] testFloat -- 1.2
         2018-12-03 11:40:49.114216+0800 TestNumber[17083:101991] testFloat -- 1.2
         2018-12-03 11:40:49.114446+0800 TestNumber[17083:101991] testFloat -- 1.2
         */
    }
    
    - (void)testRoundF
    {
        // 四舍五入
        CGFloat f0 = 1.1;
        CGFloat f1 = 1.11;
        CGFloat f2 = 1.12;
        CGFloat f3 = 1.13;
        CGFloat f4 = 1.14;
        CGFloat f5 = 1.15;
        CGFloat f6 = 1.16;
        CGFloat f7 = 1.17;
        CGFloat f8 = 1.18;
        CGFloat f9 = 1.19;
        NSLog(@"testRoundF -- %.1f", roundf(f0 * 10) / 10);
        NSLog(@"testRoundF -- %.1f", roundf(f1 * 10) / 10);
        NSLog(@"testRoundF -- %.1f", roundf(f2 * 10) / 10);
        NSLog(@"testRoundF -- %.1f", roundf(f3 * 10) / 10);
        NSLog(@"testRoundF -- %.1f", roundf(f4 * 10) / 10);
        NSLog(@"testRoundF -- %.1f", roundf(f5 * 10) / 10);
        NSLog(@"testRoundF -- %.1f", roundf(f6 * 10) / 10);
        NSLog(@"testRoundF -- %.1f", roundf(f7 * 10) / 10);
        NSLog(@"testRoundF -- %.1f", roundf(f8 * 10) / 10);
        NSLog(@"testRoundF -- %.1f", roundf(f9 * 10) / 10);
        
        // 打印日志
        /*
         2018-12-03 14:27:52.168818+0800 TestNumber[23277:179881] testRoundF -- 1.1
         2018-12-03 14:27:52.169022+0800 TestNumber[23277:179881] testRoundF -- 1.1
         2018-12-03 14:27:52.169125+0800 TestNumber[23277:179881] testRoundF -- 1.1
         2018-12-03 14:27:52.169206+0800 TestNumber[23277:179881] testRoundF -- 1.1
         2018-12-03 14:27:52.169280+0800 TestNumber[23277:179881] testRoundF -- 1.1
         2018-12-03 14:27:52.169353+0800 TestNumber[23277:179881] testRoundF -- 1.2
         2018-12-03 14:27:52.169427+0800 TestNumber[23277:179881] testRoundF -- 1.2
         2018-12-03 14:27:52.169521+0800 TestNumber[23277:179881] testRoundF -- 1.2
         2018-12-03 14:27:52.169930+0800 TestNumber[23277:179881] testRoundF -- 1.2
         2018-12-03 14:27:52.170439+0800 TestNumber[23277:179881] testRoundF -- 1.2
         */
    }
    
    - (void)testCeilf
    {
        // 向上取整
        CGFloat f0 = 1.1;
        CGFloat f1 = 1.11;
        CGFloat f2 = 1.12;
        CGFloat f3 = 1.13;
        CGFloat f4 = 1.14;
        CGFloat f5 = 1.15;
        CGFloat f6 = 1.16;
        CGFloat f7 = 1.17;
        CGFloat f8 = 1.18;
        CGFloat f9 = 1.19;
        NSLog(@"testCeilf -- %.1f", ceilf(f0 * 10) / 10);
        NSLog(@"testCeilf -- %.1f", ceilf(f1 * 10) / 10);
        NSLog(@"testCeilf -- %.1f", ceilf(f2 * 10) / 10);
        NSLog(@"testCeilf -- %.1f", ceilf(f3 * 10) / 10);
        NSLog(@"testCeilf -- %.1f", ceilf(f4 * 10) / 10);
        NSLog(@"testCeilf -- %.1f", ceilf(f5 * 10) / 10);
        NSLog(@"testCeilf -- %.1f", ceilf(f6 * 10) / 10);
        NSLog(@"testCeilf -- %.1f", ceilf(f7 * 10) / 10);
        NSLog(@"testCeilf -- %.1f", ceilf(f8 * 10) / 10);
        NSLog(@"testCeilf -- %.1f", ceilf(f9 * 10) / 10);
        
        // 打印日志
        /*
         2018-12-03 14:28:49.485549+0800 TestNumber[23326:180761] testCeilf -- 1.1
         2018-12-03 14:28:49.485749+0800 TestNumber[23326:180761] testCeilf -- 1.2
         2018-12-03 14:28:49.485834+0800 TestNumber[23326:180761] testCeilf -- 1.2
         2018-12-03 14:28:49.485909+0800 TestNumber[23326:180761] testCeilf -- 1.2
         2018-12-03 14:28:49.485982+0800 TestNumber[23326:180761] testCeilf -- 1.2
         2018-12-03 14:28:49.486059+0800 TestNumber[23326:180761] testCeilf -- 1.2
         2018-12-03 14:28:49.486177+0800 TestNumber[23326:180761] testCeilf -- 1.2
         2018-12-03 14:28:49.486264+0800 TestNumber[23326:180761] testCeilf -- 1.2
         2018-12-03 14:28:49.486523+0800 TestNumber[23326:180761] testCeilf -- 1.2
         2018-12-03 14:28:49.486731+0800 TestNumber[23326:180761] testCeilf -- 1.2
         */
    }
    
    - (void)testFloorf
    {
        // 向下取整
        CGFloat f0 = 1.1;
        CGFloat f1 = 1.11;
        CGFloat f2 = 1.12;
        CGFloat f3 = 1.13;
        CGFloat f4 = 1.14;
        CGFloat f5 = 1.15;
        CGFloat f6 = 1.16;
        CGFloat f7 = 1.17;
        CGFloat f8 = 1.18;
        CGFloat f9 = 1.19;
        NSLog(@"testFloorf -- %.1f", floorf(f0 * 10) / 10);
        NSLog(@"testFloorf -- %.1f", floorf(f1 * 10) / 10);
        NSLog(@"testFloorf -- %.1f", floorf(f2 * 10) / 10);
        NSLog(@"testFloorf -- %.1f", floorf(f3 * 10) / 10);
        NSLog(@"testFloorf -- %.1f", floorf(f4 * 10) / 10);
        NSLog(@"testFloorf -- %.1f", floorf(f5 * 10) / 10);
        NSLog(@"testFloorf -- %.1f", floorf(f6 * 10) / 10);
        NSLog(@"testFloorf -- %.1f", floorf(f7 * 10) / 10);
        NSLog(@"testFloorf -- %.1f", floorf(f8 * 10) / 10);
        NSLog(@"testFloorf -- %.1f", floorf(f9 * 10) / 10);
        
        // 打印日志
        /*
         2018-12-03 14:31:30.492177+0800 TestNumber[23434:182455] testFloorf -- 1.1
         2018-12-03 14:31:30.492370+0800 TestNumber[23434:182455] testFloorf -- 1.1
         2018-12-03 14:31:30.492465+0800 TestNumber[23434:182455] testFloorf -- 1.1
         2018-12-03 14:31:30.492550+0800 TestNumber[23434:182455] testFloorf -- 1.1
         2018-12-03 14:31:30.492631+0800 TestNumber[23434:182455] testFloorf -- 1.1
         2018-12-03 14:31:30.492702+0800 TestNumber[23434:182455] testFloorf -- 1.1
         2018-12-03 14:31:30.492773+0800 TestNumber[23434:182455] testFloorf -- 1.1
         2018-12-03 14:31:30.492885+0800 TestNumber[23434:182455] testFloorf -- 1.1
         2018-12-03 14:31:30.493288+0800 TestNumber[23434:182455] testFloorf -- 1.1
         2018-12-03 14:31:30.493656+0800 TestNumber[23434:182455] testFloorf -- 1.1
         */
    }
    
## 字符串的四舍五入、向上取整、向下取整

    #pragma mark - String 四舍五入、向上取整、向下取整
    - (void)testRoundCeiling
    {
        // 向上取整
        NSString *s10 = @"1.10";
        NSString *s11 = @"1.11";
        NSString *s12 = @"1.12";
        NSString *s13 = @"1.13";
        NSString *s14 = @"1.14";
        NSString *s15 = @"1.15";
        NSString *s16 = @"1.16";
        NSString *s17 = @"1.17";
        NSString *s18 = @"1.18";
        NSString *s19 = @"1.19";
        
        NSLog(@"testRoundCeiling -- %@", [self roundCeilingString:s10]);
        NSLog(@"testRoundCeiling -- %@", [self roundCeilingString:s11]);
        NSLog(@"testRoundCeiling -- %@", [self roundCeilingString:s12]);
        NSLog(@"testRoundCeiling -- %@", [self roundCeilingString:s13]);
        NSLog(@"testRoundCeiling -- %@", [self roundCeilingString:s14]);
        NSLog(@"testRoundCeiling -- %@", [self roundCeilingString:s15]);
        NSLog(@"testRoundCeiling -- %@", [self roundCeilingString:s16]);
        NSLog(@"testRoundCeiling -- %@", [self roundCeilingString:s17]);
        NSLog(@"testRoundCeiling -- %@", [self roundCeilingString:s18]);
        NSLog(@"testRoundCeiling -- %@", [self roundCeilingString:s19]);
        
        // 打印日志
        /*\
         2018-12-03 14:45:31.039511+0800 TestNumber[23914:189220] testRoundCeiling -- 1.2
         2018-12-03 14:45:31.039798+0800 TestNumber[23914:189220] testRoundCeiling -- 1.2
         2018-12-03 14:45:31.040077+0800 TestNumber[23914:189220] testRoundCeiling -- 1.2
         2018-12-03 14:45:31.040301+0800 TestNumber[23914:189220] testRoundCeiling -- 1.2
         2018-12-03 14:45:31.040484+0800 TestNumber[23914:189220] testRoundCeiling -- 1.2
         2018-12-03 14:45:31.040634+0800 TestNumber[23914:189220] testRoundCeiling -- 1.2
         2018-12-03 14:45:31.040791+0800 TestNumber[23914:189220] testRoundCeiling -- 1.2
         2018-12-03 14:45:31.040971+0800 TestNumber[23914:189220] testRoundCeiling -- 1.2
         2018-12-03 14:45:31.041260+0800 TestNumber[23914:189220] testRoundCeiling -- 1.2
         2018-12-03 14:45:31.041481+0800 TestNumber[23914:189220] testRoundCeiling -- 1.2
         */
    }
    // 向上取整
    - (NSString *)roundCeilingString:(NSString *)s
    {
        NSNumber *number = [self string2Number:s];
        
        NSNumberFormatter *formatter = [[NSNumberFormatter alloc] init];
        formatter.numberStyle = NSNumberFormatterDecimalStyle;
            formatter.maximumFractionDigits = 1; // 最多 保留一位
        formatter.roundingMode = NSNumberFormatterRoundCeiling; // 向上取整
        
        
        NSString *str = [formatter stringFromNumber:number];
        return str;
    }
    
    - (void)testRoundFloor
    {
        // 向下取整
        NSString *s10 = @"1.10";
        NSString *s11 = @"1.11";
        NSString *s12 = @"1.12";
        NSString *s13 = @"1.13";
        NSString *s14 = @"1.14";
        NSString *s15 = @"1.15";
        NSString *s16 = @"1.16";
        NSString *s17 = @"1.17";
        NSString *s18 = @"1.18";
        NSString *s19 = @"1.19";
        
        NSLog(@"testRoundFloor -- %@", [self roundFloorString:s10]);
        NSLog(@"testRoundFloor -- %@", [self roundFloorString:s11]);
        NSLog(@"testRoundFloor -- %@", [self roundFloorString:s12]);
        NSLog(@"testRoundFloor -- %@", [self roundFloorString:s13]);
        NSLog(@"testRoundFloor -- %@", [self roundFloorString:s14]);
        NSLog(@"testRoundFloor -- %@", [self roundFloorString:s15]);
        NSLog(@"testRoundFloor -- %@", [self roundFloorString:s16]);
        NSLog(@"testRoundFloor -- %@", [self roundFloorString:s17]);
        NSLog(@"testRoundFloor -- %@", [self roundFloorString:s18]);
        NSLog(@"testRoundFloor -- %@", [self roundFloorString:s19]);
        
        // 打印日志
        /*\
         2018-12-03 14:57:38.489686+0800 TestNumber[24377:195755] testRoundFloor -- 1.1
         2018-12-03 14:57:38.489967+0800 TestNumber[24377:195755] testRoundFloor -- 1.1
         2018-12-03 14:57:38.490152+0800 TestNumber[24377:195755] testRoundFloor -- 1.1
         2018-12-03 14:57:38.490334+0800 TestNumber[24377:195755] testRoundFloor -- 1.1
         2018-12-03 14:57:38.490985+0800 TestNumber[24377:195755] testRoundFloor -- 1.1
         2018-12-03 14:57:38.491238+0800 TestNumber[24377:195755] testRoundFloor -- 1.1
         2018-12-03 14:57:38.491459+0800 TestNumber[24377:195755] testRoundFloor -- 1.1
         2018-12-03 14:57:38.491658+0800 TestNumber[24377:195755] testRoundFloor -- 1.1
         2018-12-03 14:57:38.492071+0800 TestNumber[24377:195755] testRoundFloor -- 1.1
         2018-12-03 14:57:38.492277+0800 TestNumber[24377:195755] testRoundFloor -- 1.1
         */
    }
    // 向下取整
    - (NSString *)roundFloorString:(NSString *)s
    {
        NSNumber *number = [self string2Number:s];
        
        NSNumberFormatter *formatter = [[NSNumberFormatter alloc] init];
        formatter.numberStyle = NSNumberFormatterDecimalStyle;
        formatter.maximumFractionDigits = 1; // 最多 保留一位
        formatter.roundingMode = NSNumberFormatterRoundFloor; // 向下取整
        
        
        NSString *str = [formatter stringFromNumber:number];
        return str;
    }
    
    - (void)testRoundMy
    {
        // 四舍五入
        NSString *s10 = @"1.10";
        NSString *s11 = @"1.11";
        NSString *s12 = @"1.12";
        NSString *s13 = @"1.13";
        NSString *s14 = @"1.14";
        NSString *s15 = @"1.15";
        NSString *s16 = @"1.16";
        NSString *s17 = @"1.17";
        NSString *s18 = @"1.18";
        NSString *s19 = @"1.19";
        
        NSLog(@"testRoundMy -- %@", [self roundMyString:s10]);
        NSLog(@"testRoundMy -- %@", [self roundMyString:s11]);
        NSLog(@"testRoundMy -- %@", [self roundMyString:s12]);
        NSLog(@"testRoundMy -- %@", [self roundMyString:s13]);
        NSLog(@"testRoundMy -- %@", [self roundMyString:s14]);
        NSLog(@"testRoundMy -- %@", [self roundMyString:s15]);
        NSLog(@"testRoundMy -- %@", [self roundMyString:s16]);
        NSLog(@"testRoundMy -- %@", [self roundMyString:s17]);
        NSLog(@"testRoundMy -- %@", [self roundMyString:s18]);
        NSLog(@"testRoundMy -- %@", [self roundMyString:s19]);
        
        // 打印日志
        /*
         2018-12-03 15:01:58.104617+0800 TestNumber[24557:198673] testRoundMy -- 1.1
         2018-12-03 15:01:58.104923+0800 TestNumber[24557:198673] testRoundMy -- 1.1
         2018-12-03 15:01:58.105311+0800 TestNumber[24557:198673] testRoundMy -- 1.1
         2018-12-03 15:01:58.105623+0800 TestNumber[24557:198673] testRoundMy -- 1.1
         2018-12-03 15:01:58.105857+0800 TestNumber[24557:198673] testRoundMy -- 1.1
         2018-12-03 15:01:58.106093+0800 TestNumber[24557:198673] testRoundMy -- 1.2
         2018-12-03 15:01:58.106285+0800 TestNumber[24557:198673] testRoundMy -- 1.2
         2018-12-03 15:01:58.106645+0800 TestNumber[24557:198673] testRoundMy -- 1.2
         2018-12-03 15:01:58.106871+0800 TestNumber[24557:198673] testRoundMy -- 1.2
         2018-12-03 15:01:58.107442+0800 TestNumber[24557:198673] testRoundMy -- 1.2
         */
    }
    // 四舍五入
    - (NSString *)roundMyString:(NSString *)s
    {
        NSDecimalNumber *number1 = [[NSDecimalNumber alloc] initWithString:s];
        NSDecimalNumber *number2 = [[NSDecimalNumber alloc] initWithString:@"0.05"]; // 这里保留一位，所以加个 0.05
        NSDecimalNumber *r = [number1 decimalNumberByAdding:number2];
        
        return [self roundFloorString:[r stringValue]];
    }
    
## 保留1位小数
代码如下：

    #pragma mark - 保留1位小数
    - (void)testRetainOnePointString
    {
        NSString *s10 = @"1.10";
        NSString *s11 = @"1.11";
        NSString *s12 = @"1.12";
        NSString *s13 = @"1.13";
        NSString *s14 = @"1.14";
        NSString *s15 = @"1.15";
        NSString *s16 = @"1.16";
        NSString *s17 = @"1.17";
        NSString *s18 = @"1.18";
        NSString *s19 = @"1.19";
        
        NSLog(@"testRetainOnePointString -- %@", [self remainOnePointString:s10]);
        NSLog(@"testRetainOnePointString -- %@", [self remainOnePointString:s11]);
        NSLog(@"testRetainOnePointString -- %@", [self remainOnePointString:s12]);
        NSLog(@"testRetainOnePointString -- %@", [self remainOnePointString:s13]);
        NSLog(@"testRetainOnePointString -- %@", [self remainOnePointString:s14]);
        NSLog(@"testRetainOnePointString -- %@", [self remainOnePointString:s15]);
        NSLog(@"testRetainOnePointString -- %@", [self remainOnePointString:s16]);
        NSLog(@"testRetainOnePointString -- %@", [self remainOnePointString:s17]);
        NSLog(@"testRetainOnePointString -- %@", [self remainOnePointString:s18]);
        NSLog(@"testRetainOnePointString -- %@", [self remainOnePointString:s19]);
        
        // 打印日志：
        /*
         2018-12-03 11:29:21.256296+0800 TestNumber[16615:94362] testRetainOnePointString -- 1.1
         2018-12-03 11:29:21.256580+0800 TestNumber[16615:94362] testRetainOnePointString -- 1.1
         2018-12-03 11:29:21.256743+0800 TestNumber[16615:94362] testRetainOnePointString -- 1.1
         2018-12-03 11:29:21.256892+0800 TestNumber[16615:94362] testRetainOnePointString -- 1.1
         2018-12-03 11:29:21.257027+0800 TestNumber[16615:94362] testRetainOnePointString -- 1.1
         2018-12-03 11:29:21.257141+0800 TestNumber[16615:94362] testRetainOnePointString -- 1.1
         2018-12-03 11:29:21.257267+0800 TestNumber[16615:94362] testRetainOnePointString -- 1.2
         2018-12-03 11:29:21.257386+0800 TestNumber[16615:94362] testRetainOnePointString -- 1.2
         2018-12-03 11:29:21.257489+0800 TestNumber[16615:94362] testRetainOnePointString -- 1.2
         2018-12-03 11:29:21.257593+0800 TestNumber[16615:94362] testRetainOnePointString -- 1.2
         */
    }
    
    - (void)testRetainOnePointNumber
    {
        NSString *s10 = @"1.10";
        NSString *s11 = @"1.11";
        NSString *s12 = @"1.12";
        NSString *s13 = @"1.13";
        NSString *s14 = @"1.14";
        NSString *s15 = @"1.15";
        NSString *s16 = @"1.16";
        NSString *s17 = @"1.17";
        NSString *s18 = @"1.18";
        NSString *s19 = @"1.19";
        
        NSLog(@"testRetainOnePointNumber -- %@", [self remainOnePointNumber:[self string2Number:s10]]);
        NSLog(@"testRetainOnePointNumber -- %@", [self remainOnePointNumber:[self string2Number:s11]]);
        NSLog(@"testRetainOnePointNumber -- %@", [self remainOnePointNumber:[self string2Number:s12]]);
        NSLog(@"testRetainOnePointNumber -- %@", [self remainOnePointNumber:[self string2Number:s13]]);
        NSLog(@"testRetainOnePointNumber -- %@", [self remainOnePointNumber:[self string2Number:s14]]);
        NSLog(@"testRetainOnePointNumber -- %@", [self remainOnePointNumber:[self string2Number:s15]]);
        NSLog(@"testRetainOnePointNumber -- %@", [self remainOnePointNumber:[self string2Number:s16]]);
        NSLog(@"testRetainOnePointNumber -- %@", [self remainOnePointNumber:[self string2Number:s17]]);
        NSLog(@"testRetainOnePointNumber -- %@", [self remainOnePointNumber:[self string2Number:s18]]);
        NSLog(@"testRetainOnePointNumber -- %@", [self remainOnePointNumber:[self string2Number:s19]]);
        
        // 打印日志：
        /*
         2018-12-03 11:33:04.879571+0800 TestNumber[16783:97155] testRetainOnePointNumber -- 1.1
         2018-12-03 11:33:04.879793+0800 TestNumber[16783:97155] testRetainOnePointNumber -- 1.1
         2018-12-03 11:33:04.879911+0800 TestNumber[16783:97155] testRetainOnePointNumber -- 1.1
         2018-12-03 11:33:04.880031+0800 TestNumber[16783:97155] testRetainOnePointNumber -- 1.1
         2018-12-03 11:33:04.880219+0800 TestNumber[16783:97155] testRetainOnePointNumber -- 1.1
         2018-12-03 11:33:04.880367+0800 TestNumber[16783:97155] testRetainOnePointNumber -- 1.1
         2018-12-03 11:33:04.880484+0800 TestNumber[16783:97155] testRetainOnePointNumber -- 1.2
         2018-12-03 11:33:04.880603+0800 TestNumber[16783:97155] testRetainOnePointNumber -- 1.2
         2018-12-03 11:33:04.880958+0800 TestNumber[16783:97155] testRetainOnePointNumber -- 1.2
         2018-12-03 11:33:04.881180+0800 TestNumber[16783:97155] testRetainOnePointNumber -- 1.2
         */
    }
    
    - (void)testRetainOnePointNumber1
    {
        NSString *s10 = @"1.10";
        NSString *s11 = @"1.11";
        NSString *s12 = @"1.12";
        NSString *s13 = @"1.13";
        NSString *s14 = @"1.14";
        NSString *s15 = @"1.15";
        NSString *s16 = @"1.16";
        NSString *s17 = @"1.17";
        NSString *s18 = @"1.18";
        NSString *s19 = @"1.19";
        
        NSLog(@"testRetainOnePointNumber1 -- %@", [self remainOnePointNumber1:[self string2Number:s10]]);
        NSLog(@"testRetainOnePointNumber1 -- %@", [self remainOnePointNumber1:[self string2Number:s11]]);
        NSLog(@"testRetainOnePointNumber1 -- %@", [self remainOnePointNumber1:[self string2Number:s12]]);
        NSLog(@"testRetainOnePointNumber1 -- %@", [self remainOnePointNumber1:[self string2Number:s13]]);
        NSLog(@"testRetainOnePointNumber1 -- %@", [self remainOnePointNumber1:[self string2Number:s14]]);
        NSLog(@"testRetainOnePointNumber1 -- %@", [self remainOnePointNumber1:[self string2Number:s15]]);
        NSLog(@"testRetainOnePointNumber1 -- %@", [self remainOnePointNumber1:[self string2Number:s16]]);
        NSLog(@"testRetainOnePointNumber1 -- %@", [self remainOnePointNumber1:[self string2Number:s17]]);
        NSLog(@"testRetainOnePointNumber1 -- %@", [self remainOnePointNumber1:[self string2Number:s18]]);
        NSLog(@"testRetainOnePointNumber1 -- %@", [self remainOnePointNumber1:[self string2Number:s19]]);
        
        // 打印日志：
        /*
         2018-12-03 15:11:31.583702+0800 TestNumber[24938:204083] testRetainOnePointNumber1 -- 1.1
         2018-12-03 15:11:31.584127+0800 TestNumber[24938:204083] testRetainOnePointNumber1 -- 1.1
         2018-12-03 15:11:31.584480+0800 TestNumber[24938:204083] testRetainOnePointNumber1 -- 1.1
         2018-12-03 15:11:31.584752+0800 TestNumber[24938:204083] testRetainOnePointNumber1 -- 1.1
         2018-12-03 15:11:31.585034+0800 TestNumber[24938:204083] testRetainOnePointNumber1 -- 1.1
         2018-12-03 15:11:31.585349+0800 TestNumber[24938:204083] testRetainOnePointNumber1 -- 1.1
         2018-12-03 15:11:31.585584+0800 TestNumber[24938:204083] testRetainOnePointNumber1 -- 1.2
         2018-12-03 15:11:31.586180+0800 TestNumber[24938:204083] testRetainOnePointNumber1 -- 1.2
         2018-12-03 15:11:31.586708+0800 TestNumber[24938:204083] testRetainOnePointNumber1 -- 1.2
         2018-12-03 15:11:31.587225+0800 TestNumber[24938:204083] testRetainOnePointNumber1 -- 1.2
         */
    }
    
    // 保留一位小数
    - (NSString *)remainOnePointString:(NSString *)s
    {
        NSString *string = [NSString stringWithFormat:@"%.1f", [s floatValue]];
        return string;
    }
    - (NSString *)remainOnePointNumber:(NSNumber *)number
    {
        NSString *string = [NSString stringWithFormat:@"%.1f", [number floatValue]];
        return string;
    }
    - (NSString *)remainOnePointNumber1:(NSNumber *)number
    {
        NSNumberFormatter *formatter = [[NSNumberFormatter alloc] init];
        formatter.maximumFractionDigits = 1; // 保留一位小数
        return [formatter stringFromNumber:number];
    }
    
## 保留三位小数
代码如下：

    #pragma mark - 保留两三位小数
    - (void)testRetainThreePointString
    {
        NSString *s10 = @"1.10";
        NSString *s11 = @"1.11";
        NSString *s12 = @"1.12";
        NSString *s13 = @"1.13";
        NSString *s14 = @"1.14";
        NSString *s15 = @"1.15";
        NSString *s16 = @"1.16";
        NSString *s17 = @"1.17";
        NSString *s18 = @"1.18";
        NSString *s19 = @"1.19";
        
        NSLog(@"testRetainThreePointString -- %@", [self remainThreePointString:s10]);
        NSLog(@"testRetainThreePointString -- %@", [self remainThreePointString:s11]);
        NSLog(@"testRetainThreePointString -- %@", [self remainThreePointString:s12]);
        NSLog(@"testRetainThreePointString -- %@", [self remainThreePointString:s13]);
        NSLog(@"testRetainThreePointString -- %@", [self remainThreePointString:s14]);
        NSLog(@"testRetainThreePointString -- %@", [self remainThreePointString:s15]);
        NSLog(@"testRetainThreePointString -- %@", [self remainThreePointString:s16]);
        NSLog(@"testRetainThreePointString -- %@", [self remainThreePointString:s17]);
        NSLog(@"testRetainThreePointString -- %@", [self remainThreePointString:s18]);
        NSLog(@"testRetainThreePointString -- %@", [self remainThreePointString:s19]);
        
        // 打印日志：
        /*
         2018-12-03 15:25:31.550660+0800 TestNumber[25509:212948] testRetainThreePointString -- 1.100
         2018-12-03 15:25:31.550852+0800 TestNumber[25509:212948] testRetainThreePointString -- 1.110
         2018-12-03 15:25:31.550986+0800 TestNumber[25509:212948] testRetainThreePointString -- 1.120
         2018-12-03 15:25:31.551098+0800 TestNumber[25509:212948] testRetainThreePointString -- 1.130
         2018-12-03 15:25:31.551219+0800 TestNumber[25509:212948] testRetainThreePointString -- 1.140
         2018-12-03 15:25:31.551357+0800 TestNumber[25509:212948] testRetainThreePointString -- 1.150
         2018-12-03 15:25:31.551473+0800 TestNumber[25509:212948] testRetainThreePointString -- 1.160
         2018-12-03 15:25:31.551653+0800 TestNumber[25509:212948] testRetainThreePointString -- 1.170
         2018-12-03 15:25:31.552031+0800 TestNumber[25509:212948] testRetainThreePointString -- 1.180
         2018-12-03 15:25:31.552391+0800 TestNumber[25509:212948] testRetainThreePointString -- 1.190
         */
    }
    
    - (void)testRetainThreePointNumber
    {
        NSString *s10 = @"1.10";
        NSString *s11 = @"1.11";
        NSString *s12 = @"1.12";
        NSString *s13 = @"1.13";
        NSString *s14 = @"1.14";
        NSString *s15 = @"1.15";
        NSString *s16 = @"1.16";
        NSString *s17 = @"1.17";
        NSString *s18 = @"1.18";
        NSString *s19 = @"1.19";
        
        NSLog(@"testRetainThreePointNumber -- %@", [self remainThreePointNumber:[self string2Number:s10]]);
        NSLog(@"testRetainThreePointNumber -- %@", [self remainThreePointNumber:[self string2Number:s11]]);
        NSLog(@"testRetainThreePointNumber -- %@", [self remainThreePointNumber:[self string2Number:s12]]);
        NSLog(@"testRetainThreePointNumber -- %@", [self remainThreePointNumber:[self string2Number:s13]]);
        NSLog(@"testRetainThreePointNumber -- %@", [self remainThreePointNumber:[self string2Number:s14]]);
        NSLog(@"testRetainThreePointNumber -- %@", [self remainThreePointNumber:[self string2Number:s15]]);
        NSLog(@"testRetainThreePointNumber -- %@", [self remainThreePointNumber:[self string2Number:s16]]);
        NSLog(@"testRetainThreePointNumber -- %@", [self remainThreePointNumber:[self string2Number:s17]]);
        NSLog(@"testRetainThreePointNumber -- %@", [self remainThreePointNumber:[self string2Number:s18]]);
        NSLog(@"testRetainThreePointNumber -- %@", [self remainThreePointNumber:[self string2Number:s19]]);
        
        // 打印日志：
        /*
         2018-12-03 15:26:02.436907+0800 TestNumber[25533:213455] testRetainThreePointNumber -- 1.100
         2018-12-03 15:26:02.437162+0800 TestNumber[25533:213455] testRetainThreePointNumber -- 1.110
         2018-12-03 15:26:02.437311+0800 TestNumber[25533:213455] testRetainThreePointNumber -- 1.120
         2018-12-03 15:26:02.437431+0800 TestNumber[25533:213455] testRetainThreePointNumber -- 1.130
         2018-12-03 15:26:02.437577+0800 TestNumber[25533:213455] testRetainThreePointNumber -- 1.140
         2018-12-03 15:26:02.437683+0800 TestNumber[25533:213455] testRetainThreePointNumber -- 1.150
         2018-12-03 15:26:02.437790+0800 TestNumber[25533:213455] testRetainThreePointNumber -- 1.160
         2018-12-03 15:26:02.437923+0800 TestNumber[25533:213455] testRetainThreePointNumber -- 1.170
         2018-12-03 15:26:02.438242+0800 TestNumber[25533:213455] testRetainThreePointNumber -- 1.180
         2018-12-03 15:26:02.438613+0800 TestNumber[25533:213455] testRetainThreePointNumber -- 1.190
         */
    }
    
    - (void)testRetainThreePointNumber1
    {
        NSString *s10 = @"1.10";
        NSString *s11 = @"1.11";
        NSString *s12 = @"1.12";
        NSString *s13 = @"1.13";
        NSString *s14 = @"1.14";
        NSString *s15 = @"1.15";
        NSString *s16 = @"1.16";
        NSString *s17 = @"1.17";
        NSString *s18 = @"1.18";
        NSString *s19 = @"1.19";
        
        NSLog(@"testRetainThreePointNumber1 -- %@", [self remainThreePointNumber1:[self string2Number:s10]]);
        NSLog(@"testRetainThreePointNumber1 -- %@", [self remainThreePointNumber1:[self string2Number:s11]]);
        NSLog(@"testRetainThreePointNumber1 -- %@", [self remainThreePointNumber1:[self string2Number:s12]]);
        NSLog(@"testRetainThreePointNumber1 -- %@", [self remainThreePointNumber1:[self string2Number:s13]]);
        NSLog(@"testRetainThreePointNumber1 -- %@", [self remainThreePointNumber1:[self string2Number:s14]]);
        NSLog(@"testRetainThreePointNumber1 -- %@", [self remainThreePointNumber1:[self string2Number:s15]]);
        NSLog(@"testRetainThreePointNumber1 -- %@", [self remainThreePointNumber1:[self string2Number:s16]]);
        NSLog(@"testRetainThreePointNumber1 -- %@", [self remainThreePointNumber1:[self string2Number:s17]]);
        NSLog(@"testRetainThreePointNumber1 -- %@", [self remainThreePointNumber1:[self string2Number:s18]]);
        NSLog(@"testRetainThreePointNumber1 -- %@", [self remainThreePointNumber1:[self string2Number:s19]]);
        
        // 打印日志：
        /*
         2018-12-03 15:26:26.518477+0800 TestNumber[25556:213952] testRetainThreePointNumber1 -- 1.1
         2018-12-03 15:26:26.518867+0800 TestNumber[25556:213952] testRetainThreePointNumber1 -- 1.11
         2018-12-03 15:26:26.519143+0800 TestNumber[25556:213952] testRetainThreePointNumber1 -- 1.12
         2018-12-03 15:26:26.519386+0800 TestNumber[25556:213952] testRetainThreePointNumber1 -- 1.13
         2018-12-03 15:26:26.519680+0800 TestNumber[25556:213952] testRetainThreePointNumber1 -- 1.14
         2018-12-03 15:26:26.519949+0800 TestNumber[25556:213952] testRetainThreePointNumber1 -- 1.15
         2018-12-03 15:26:26.520191+0800 TestNumber[25556:213952] testRetainThreePointNumber1 -- 1.16
         2018-12-03 15:26:26.520473+0800 TestNumber[25556:213952] testRetainThreePointNumber1 -- 1.17
         2018-12-03 15:26:26.521101+0800 TestNumber[25556:213952] testRetainThreePointNumber1 -- 1.18
         2018-12-03 15:26:26.521496+0800 TestNumber[25556:213952] testRetainThreePointNumber1 -- 1.19
         */
    }
    
    // 保留三位小数
    - (NSString *)remainThreePointString:(NSString *)s
    {
        NSString *string = [NSString stringWithFormat:@"%.3f", [s floatValue]];
        return string;
    }
    - (NSString *)remainThreePointNumber:(NSNumber *)number
    {
        NSString *string = [NSString stringWithFormat:@"%.3f", [number floatValue]];
        return string;
    }
    - (NSString *)remainThreePointNumber1:(NSNumber *)number
    {
        NSNumberFormatter *formatter = [[NSNumberFormatter alloc] init];
        formatter.maximumFractionDigits = 3; // 保留三位小数
        return [formatter stringFromNumber:number];
    }
    
## 向下取整出现的误差
代码如下：

    // 向下取整 误差
    - (void)testSub1
    {
        NSString *s10 = @"1.90";
        NSString *s11 = @"1.91";
        NSString *s12 = @"1.92";
        NSString *s13 = @"1.93";
        NSString *s14 = @"1.94";
        NSString *s15 = @"1.95";
        NSString *s16 = @"1.96";
        NSString *s17 = @"1.97";
        NSString *s18 = @"1.98";
        NSString *s19 = @"1.99";
        
        NSLog(@"testSub1 -- %@", [self roundFloorString:s10]);
        NSLog(@"testSub1 -- %@", [self roundFloorString:s11]);
        NSLog(@"testSub1 -- %@", [self roundFloorString:s12]);
        NSLog(@"testSub1 -- %@", [self roundFloorString:s13]);
        NSLog(@"testSub1 -- %@", [self roundFloorString:s14]);
        NSLog(@"testSub1 -- %@", [self roundFloorString:s15]);
        NSLog(@"testSub1 -- %@", [self roundFloorString:s16]);
        NSLog(@"testSub1 -- %@", [self roundFloorString:s17]);
        NSLog(@"testSub1 -- %@", [self roundFloorString:s18]);
        NSLog(@"testSub1 -- %@", [self roundFloorString:s19]);
        
        // 打印日志
        /*
         2018-12-04 15:07:44.562198+0800 TestNumber[19420:652098] testSub1 -- 1.8
         2018-12-04 15:07:44.562486+0800 TestNumber[19420:652098] testSub1 -- 1.9
         2018-12-04 15:07:44.562767+0800 TestNumber[19420:652098] testSub1 -- 1.9
         2018-12-04 15:07:44.562979+0800 TestNumber[19420:652098] testSub1 -- 1.9
         2018-12-04 15:07:44.563183+0800 TestNumber[19420:652098] testSub1 -- 1.9
         2018-12-04 15:07:44.563377+0800 TestNumber[19420:652098] testSub1 -- 1.9
         2018-12-04 15:07:44.563575+0800 TestNumber[19420:652098] testSub1 -- 1.9
         2018-12-04 15:07:44.563790+0800 TestNumber[19420:652098] testSub1 -- 1.9
         2018-12-04 15:07:44.563983+0800 TestNumber[19420:652098] testSub1 -- 1.9
         2018-12-04 15:07:44.564181+0800 TestNumber[19420:652098] testSub1 -- 1.9
         */
    }
    
    // 向下取整 修正
    - (void)testSub2
    {
        NSString *s10 = @"1.90";
        NSString *s11 = @"1.91";
        NSString *s12 = @"1.92";
        NSString *s13 = @"1.93";
        NSString *s14 = @"1.94";
        NSString *s15 = @"1.95";
        NSString *s16 = @"1.96";
        NSString *s17 = @"1.97";
        NSString *s18 = @"1.98";
        NSString *s19 = @"1.99";
        
        NSLog(@"testSub2 -- %@", [self roundFloorString1:s10]);
        NSLog(@"testSub2 -- %@", [self roundFloorString1:s11]);
        NSLog(@"testSub2 -- %@", [self roundFloorString1:s12]);
        NSLog(@"testSub2 -- %@", [self roundFloorString1:s13]);
        NSLog(@"testSub2 -- %@", [self roundFloorString1:s14]);
        NSLog(@"testSub2 -- %@", [self roundFloorString1:s15]);
        NSLog(@"testSub2 -- %@", [self roundFloorString1:s16]);
        NSLog(@"testSub2 -- %@", [self roundFloorString1:s17]);
        NSLog(@"testSub2 -- %@", [self roundFloorString1:s18]);
        NSLog(@"testSub2 -- %@", [self roundFloorString1:s19]);
        
        // 打印日志
        /*
         2018-12-04 15:09:05.571962+0800 TestNumber[19482:653134] testSub2 -- 1.9
         2018-12-04 15:09:05.572267+0800 TestNumber[19482:653134] testSub2 -- 1.9
         2018-12-04 15:09:05.572495+0800 TestNumber[19482:653134] testSub2 -- 1.9
         2018-12-04 15:09:05.572704+0800 TestNumber[19482:653134] testSub2 -- 1.9
         2018-12-04 15:09:05.572970+0800 TestNumber[19482:653134] testSub2 -- 1.9
         2018-12-04 15:09:05.573229+0800 TestNumber[19482:653134] testSub2 -- 1.9
         2018-12-04 15:09:05.573624+0800 TestNumber[19482:653134] testSub2 -- 1.9
         2018-12-04 15:09:05.573864+0800 TestNumber[19482:653134] testSub2 -- 1.9
         2018-12-04 15:09:05.574131+0800 TestNumber[19482:653134] testSub2 -- 1.9
         2018-12-04 15:09:05.574468+0800 TestNumber[19482:653134] testSub2 -- 1.9
         */
    }
    
    // 向下取整 修正
    - (NSString *)roundFloorString1:(NSString *)s
    {
        NSNumber *number = [self string2Number:s];
        
        NSNumberFormatter *formatter = [[NSNumberFormatter alloc] init];
        formatter.numberStyle = NSNumberFormatterDecimalStyle;
        formatter.maximumFractionDigits = 1; // 最多 保留一位
        formatter.roundingMode = NSNumberFormatterRoundFloor; // 向下取整
        
        
        NSString *str = [formatter stringFromNumber:@([number floatValue] + 0.000001)]; // 消除误差
        return str;
    }
    
    // 向下取整 计算过程出现误差
    - (void)testSub3
    {
        NSString *s10 = @"1.90";
        NSString *s11 = @"1.91";
        NSString *s12 = @"1.92";
        NSString *s13 = @"1.93";
        NSString *s14 = @"1.94";
        NSString *s15 = @"1.95";
        NSString *s16 = @"1.96";
        NSString *s17 = @"1.97";
        NSString *s18 = @"1.98";
        NSString *s19 = @"1.99";
        
        NSLog(@"testSub3 -- %@", [self roundFloorFloat:[s10 floatValue]]);
        NSLog(@"testSub3 -- %@", [self roundFloorFloat:[s11 floatValue]]);
        NSLog(@"testSub3 -- %@", [self roundFloorFloat:[s12 floatValue]]);
        NSLog(@"testSub3 -- %@", [self roundFloorFloat:[s13 floatValue]]);
        NSLog(@"testSub3 -- %@", [self roundFloorFloat:[s14 floatValue]]);
        NSLog(@"testSub3 -- %@", [self roundFloorFloat:[s15 floatValue]]);
        NSLog(@"testSub3 -- %@", [self roundFloorFloat:[s16 floatValue]]);
        NSLog(@"testSub3 -- %@", [self roundFloorFloat:[s17 floatValue]]);
        NSLog(@"testSub3 -- %@", [self roundFloorFloat:[s18 floatValue]]);
        NSLog(@"testSub3 -- %@", [self roundFloorFloat:[s19 floatValue]]);
        
        // 打印日志
        /*
         2018-12-04 15:25:59.925060+0800 TestNumber[20129:663532] ssss -- 1.399999976158142
         2018-12-04 15:25:59.925369+0800 TestNumber[20129:663532] testSub3 -- 1.3
         2018-12-04 15:25:59.925503+0800 TestNumber[20129:663532] ssss -- 1.409999966621399
         2018-12-04 15:25:59.925691+0800 TestNumber[20129:663532] testSub3 -- 1.4
         2018-12-04 15:25:59.925799+0800 TestNumber[20129:663532] ssss -- 1.419999957084656
         2018-12-04 15:25:59.925966+0800 TestNumber[20129:663532] testSub3 -- 1.4
         2018-12-04 15:25:59.926086+0800 TestNumber[20129:663532] ssss -- 1.429999947547913
         2018-12-04 15:25:59.926270+0800 TestNumber[20129:663532] testSub3 -- 1.4
         2018-12-04 15:25:59.926394+0800 TestNumber[20129:663532] ssss -- 1.440000057220459
         2018-12-04 15:25:59.926572+0800 TestNumber[20129:663532] testSub3 -- 1.4
         2018-12-04 15:25:59.926687+0800 TestNumber[20129:663532] ssss -- 1.450000047683716
         2018-12-04 15:25:59.926860+0800 TestNumber[20129:663532] testSub3 -- 1.4
         2018-12-04 15:25:59.932725+0800 TestNumber[20129:663532] ssss -- 1.460000038146973
         2018-12-04 15:25:59.932947+0800 TestNumber[20129:663532] testSub3 -- 1.4
         2018-12-04 15:25:59.933071+0800 TestNumber[20129:663532] ssss -- 1.470000028610229
         2018-12-04 15:25:59.933235+0800 TestNumber[20129:663532] testSub3 -- 1.4
         2018-12-04 15:25:59.933344+0800 TestNumber[20129:663532] ssss -- 1.480000019073486
         2018-12-04 15:25:59.933508+0800 TestNumber[20129:663532] testSub3 -- 1.4
         2018-12-04 15:25:59.933608+0800 TestNumber[20129:663532] ssss -- 1.490000009536743
         2018-12-04 15:25:59.933814+0800 TestNumber[20129:663532] testSub3 -- 1.4
         */
    }
    
    // 向下取整
    - (NSString *)roundFloorFloat:(CGFloat)f
    {
        CGFloat sub = 0.5;
        NSNumber *number =  @(f - sub); // 做个计算，出现误差
        NSLog(@"ssss -- %@", number);
        
        NSNumberFormatter *formatter = [[NSNumberFormatter alloc] init];
        formatter.numberStyle = NSNumberFormatterDecimalStyle;
        formatter.maximumFractionDigits = 1; // 最多 保留一位
        formatter.roundingMode = NSNumberFormatterRoundFloor; // 向下取整
        
        NSString *str = [formatter stringFromNumber:@([number floatValue])];
        return str;
    }
    
    // 向下取整 计算过程出现误差 修正
    - (void)testSub4
    {
        NSString *s10 = @"1.90";
        NSString *s11 = @"1.91";
        NSString *s12 = @"1.92";
        NSString *s13 = @"1.93";
        NSString *s14 = @"1.94";
        NSString *s15 = @"1.95";
        NSString *s16 = @"1.96";
        NSString *s17 = @"1.97";
        NSString *s18 = @"1.98";
        NSString *s19 = @"1.99";
        
        NSLog(@"testSub4 -- %@", [self roundFloorFloat1:[s10 floatValue]]);
        NSLog(@"testSub4 -- %@", [self roundFloorFloat1:[s11 floatValue]]);
        NSLog(@"testSub4 -- %@", [self roundFloorFloat1:[s12 floatValue]]);
        NSLog(@"testSub4 -- %@", [self roundFloorFloat1:[s13 floatValue]]);
        NSLog(@"testSub4 -- %@", [self roundFloorFloat1:[s14 floatValue]]);
        NSLog(@"testSub4 -- %@", [self roundFloorFloat1:[s15 floatValue]]);
        NSLog(@"testSub4 -- %@", [self roundFloorFloat1:[s16 floatValue]]);
        NSLog(@"testSub4 -- %@", [self roundFloorFloat1:[s17 floatValue]]);
        NSLog(@"testSub4 -- %@", [self roundFloorFloat1:[s18 floatValue]]);
        NSLog(@"testSub4 -- %@", [self roundFloorFloat1:[s19 floatValue]]);
        
        // 打印日志
        /*
         2018-12-04 15:28:24.076919+0800 TestNumber[20228:665143] ssss -- 1.399999976158142
         2018-12-04 15:28:24.077252+0800 TestNumber[20228:665143] testSub4 -- 1.4
         2018-12-04 15:28:24.077396+0800 TestNumber[20228:665143] ssss -- 1.409999966621399
         2018-12-04 15:28:24.077580+0800 TestNumber[20228:665143] testSub4 -- 1.4
         2018-12-04 15:28:24.077684+0800 TestNumber[20228:665143] ssss -- 1.419999957084656
         2018-12-04 15:28:24.077852+0800 TestNumber[20228:665143] testSub4 -- 1.4
         2018-12-04 15:28:24.077943+0800 TestNumber[20228:665143] ssss -- 1.429999947547913
         2018-12-04 15:28:24.078108+0800 TestNumber[20228:665143] testSub4 -- 1.4
         2018-12-04 15:28:24.078353+0800 TestNumber[20228:665143] ssss -- 1.440000057220459
         2018-12-04 15:28:24.078895+0800 TestNumber[20228:665143] testSub4 -- 1.4
         2018-12-04 15:28:24.079059+0800 TestNumber[20228:665143] ssss -- 1.450000047683716
         2018-12-04 15:28:24.079513+0800 TestNumber[20228:665143] testSub4 -- 1.4
         2018-12-04 15:28:24.079847+0800 TestNumber[20228:665143] ssss -- 1.460000038146973
         2018-12-04 15:28:24.080295+0800 TestNumber[20228:665143] testSub4 -- 1.4
         2018-12-04 15:28:24.080660+0800 TestNumber[20228:665143] ssss -- 1.470000028610229
         2018-12-04 15:28:24.081062+0800 TestNumber[20228:665143] testSub4 -- 1.4
         2018-12-04 15:28:24.081191+0800 TestNumber[20228:665143] ssss -- 1.480000019073486
         2018-12-04 15:28:24.081611+0800 TestNumber[20228:665143] testSub4 -- 1.4
         2018-12-04 15:28:24.081957+0800 TestNumber[20228:665143] ssss -- 1.490000009536743
         2018-12-04 15:28:24.082355+0800 TestNumber[20228:665143] testSub4 -- 1.4
         */
    }
    
    // 向下取整
    - (NSString *)roundFloorFloat1:(CGFloat)f
    {
        CGFloat sub = 0.5;
        NSNumber *number =  @(f - sub); // 做个计算，出现误差
        NSLog(@"ssss -- %@", number);
        
        NSNumberFormatter *formatter = [[NSNumberFormatter alloc] init];
        formatter.numberStyle = NSNumberFormatterDecimalStyle;
        formatter.maximumFractionDigits = 1; // 最多 保留一位
        formatter.roundingMode = NSNumberFormatterRoundFloor; // 向下取整
        
        
        NSString *str = [formatter stringFromNumber:@([number floatValue] + 0.000001)]; // 消除误差
        return str;
    }


以上，就是各个情况的测试用例，测试代码在[这里](https://github.com/jianghui1/TestNumber)。
