

#import "TTMUpdateAssistController.h"
#import "TTMUpdateStepper.h"
#import "TTMAssistModel.h"
#import "TTMMaterialModel.h"

@interface TTMUpdateAssistController ()<TTMUpdateSteperDelegate>

@property (nonatomic, strong) NSArray *assistArray; // 助理列表

@property (nonatomic, weak) UIView *assistListView; // 存放助手列表,种植体列表，其他列表

@property (nonatomic, weak) UILabel *countLabel; // 总计价格

@property (nonatomic, assign) NSUInteger countMoney; // 总价格
@end

@implementation TTMUpdateAssistController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    self.edgesForExtendedLayout = UIRectEdgeNone;
    if (self.type == TTMUpdateAssistControllerTypeAssist) {
        self.title = @"修改助手费用";
        [self queryAssistData];
    } else if (self.type == TTMUpdateAssistControllerTypePlant) {
        self.title = @"修改种植体费用";
        [self queryPlantData];
    }
    
    [self setupRightItem];
}

- (void)setupRightItem {
    self.navigationItem.rightBarButtonItem = [UIBarButtonItem barButtonItemWithTitle:@"确定"
                                                                     normalImageName:@"member_bar_item_bg"
                                                                              action:@selector(buttonAction:)
                                                                              target:self];
}

/**
 *  返回按钮
 *
 *  @param button button description
 */
- (void)buttonAction:(UIButton *)button {
    NSMutableArray *resultArray = [NSMutableArray array];
    if (self.type == TTMUpdateAssistControllerTypeAssist) {
        for (TTMAssistModel *model in self.assistArray) {
            if ([model.countPrice integerValue] > 0) {
                [resultArray addObject:model];
            }
        }
        if ([self.delegate respondsToSelector:@selector(updateAssistController:countMoney:assistArray:)]) {
            [self.delegate updateAssistController:self countMoney:self.countMoney assistArray:resultArray];
            [self.navigationController popViewControllerAnimated:YES];
        }
    } else if (self.type == TTMUpdateAssistControllerTypePlant) {
        for (TTMMaterialModel *model in self.assistArray) {
            if ([model.countPrice integerValue] > 0) {
                [resultArray addObject:model];
            }
        }
        if ([self.delegate respondsToSelector:@selector(updateAssistController:countMoney:plantArray:)]) {
            [self.delegate updateAssistController:self countMoney:self.countMoney plantArray:resultArray];
            [self.navigationController popViewControllerAnimated:YES];
        }
    }
    
}

- (void)setAssistArray:(NSArray *)assistArray {
    _assistArray = assistArray;
    
    UIView *assistListView = [UIView new];
    assistListView.frame = CGRectMake(0, 0, ScreenWidth, 0);
    [self.view addSubview:assistListView];
    self.assistListView = assistListView;
    
    // 添加计步器到视图上
    for (NSUInteger i = 0; i < assistArray.count; i ++) {
        id model = assistArray[i];
        TTMUpdateStepper *stepper = [TTMUpdateStepper new];
        if (self.type == TTMUpdateAssistControllerTypeAssist) {
            stepper.assistModel = model;
        } else if (self.type == TTMUpdateAssistControllerTypePlant) {
            stepper.materialModel = model;
        }
        
        stepper.delegate = self;
        stepper.top = i * 44.f;
        [self.assistListView addSubview:stepper];
        self.assistListView.height = stepper.bottom;
    }
    
    // 总计价格
    UILabel *countLabel = [UILabel new];
    countLabel.textColor = [UIColor redColor];
    countLabel.font = [UIFont systemFontOfSize:14];
    countLabel.textAlignment = NSTextAlignmentRight;
    [self.view addSubview:countLabel];
    self.countLabel = countLabel;
    
    countLabel.frame = CGRectMake(0, self.assistListView.bottom + 10.f, ScreenWidth - 10.f, 20.f);
    self.countLabel.text = [NSString stringWithFormat:@"合计 ￥%@", @"0"];
}

#pragma mark - TTMUpdateStepperDelegate
- (void)updateSteper:(TTMUpdateStepper *)updateSteper model:(TTMAssistModel *)model {
    NSUInteger countMoney = 0;
    // 计算总价
    for (TTMUpdateStepper *stepper in self.assistListView.subviews) {
        if ([stepper.assistModel.countPrice hasValue]) {
            countMoney += [stepper.assistModel.countPrice integerValue];
        }
    }
    self.countMoney = countMoney;
    self.countLabel.text = [NSString stringWithFormat:@"合计 ￥%@", @(countMoney)];
}

- (void)updateSteper:(TTMUpdateStepper *)updateSteper materialModel:(TTMMaterialModel *)materialModel {
    NSUInteger countMoney = 0;
    // 计算总价
    for (TTMUpdateStepper *stepper in self.assistListView.subviews) {
        if ([stepper.materialModel.countPrice hasValue]) {
            countMoney += [stepper.materialModel.countPrice integerValue];
        }
    }
    self.countMoney = countMoney;
    self.countLabel.text = [NSString stringWithFormat:@"合计 ￥%@", @(countMoney)];
}


/**
 *  查询助手列表
 */
- (void)queryAssistData {
    __weak __typeof(&*self) weakSelf = self;
    [TTMAssistModel queryAssistListWithComplete:^(id result) {
        if ([result isKindOfClass:[NSMutableArray class]]) {
            weakSelf.assistArray = result;
        } else {
            [MBProgressHUD showToastWithText:result];
        }
    }];
}

/**
 *  查询种植体列表
 */
- (void)queryPlantData {
    __weak __typeof(&*self) weakSelf = self;
    [TTMMaterialModel queryMaterialWithStatus:1 complete:^(id result) {
        if ([result isKindOfClass:[NSMutableArray class]]) {
            weakSelf.assistArray = result;
        } else {
            [MBProgressHUD showToastWithText:result];
        }
    }];
}
- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
}

@end
