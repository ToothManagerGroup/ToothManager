//
//  TTMScheduleDetailController.m
//  ToothManager
//

#import "TTMScheduleDetailController.h"
#import "TTMScheduleCellModel.h"
#import "TTMContractHeaderView.h"
#import "TTMInfoView.h"
#import "TTMShowTextView.h"
#import "TTMScheduleDetailInfoView.h"
#import "TTMGTaskCellModel.h"
#import "UIImage+TTMAddtion.h"
#import "TTMChargeConfirmController.h"
#import "TTMChargeDetailController.h"

#define kMarginTop 20.f

@interface TTMScheduleDetailController ()

@property (nonatomic, weak)     UIScrollView *scrollView;
@property (nonatomic, weak)     TTMContractHeaderView *headerView; //头视图
@property (nonatomic, weak)     TTMInfoView *detailInfoView; //详细信息
@property (nonatomic, weak)     TTMInfoView *seggustionInfoView; // 专家建议
@property (nonatomic, weak)     UIButton *button; // 开始计时，结束计时，等待付款，按钮

@property (nonatomic, strong)   TTMScheduleCellModel *pageModel; // 页面model
@end

@implementation TTMScheduleDetailController

- (void)viewDidLoad {
    [super viewDidLoad];
    self.title = @"预约详情";
    
    
    [self queryData];
    [self setupScrollView];
}
/**
 *  加载滚动背景
 */
- (void)setupScrollView {
    UIScrollView *scrollView = [[UIScrollView alloc] init];
    scrollView.backgroundColor = [UIColor clearColor];
    scrollView.size = CGSizeMake(ScreenWidth, ScreenHeight);
    scrollView.delaysContentTouches = NO;
    [self.view addSubview:scrollView];
    self.scrollView = scrollView;
}
/**
 *  加载所有视图
 */
- (void)setupViews {
    [self setupHeaderView];
    [self setupDetailInfoView];
    [self setupSegguestion];
    [self setupButton];
}
/**
 *  加载tableviewHeader
 */
- (void)setupHeaderView {
    TTMContractHeaderView *headerView = [[TTMContractHeaderView alloc] init];
    TTMGTaskCellModel *model = [[TTMGTaskCellModel alloc] init];
    model.doctor_image = self.pageModel.doctor_image;
    model.doctor_name = self.pageModel.doctor_name;
    model.doctor_position = self.pageModel.doctor_position;
    model.doctor_hospital = self.pageModel.doctor_hospital;
    model.doctor_dept = self.pageModel.doctor_dept;
    model.planting_quantity = self.pageModel.planting_quantity;
    model.star_level = self.pageModel.star_level;
    
    headerView.value = model;
    headerView.showInfoView = YES;
    
    [self.scrollView addSubview:headerView];
    self.headerView = headerView;
}

/**
 *  详情信息
 */
- (void)setupDetailInfoView {
    TTMScheduleDetailInfoView *infoView = [[TTMScheduleDetailInfoView alloc] initWithModel:self.pageModel];
    TTMInfoView *detailInfoView = [[TTMInfoView alloc] initWithIconName:@"gtask_doctor_another_info_icon"
                                                                  title:@"详情信息"
                                                            contentView:infoView];
    detailInfoView.origin = CGPointMake(0, self.headerView.bottom + kMarginTop);
    [self.scrollView addSubview:detailInfoView];
    self.detailInfoView = detailInfoView;
}

/**
 *  专家建议
 */
- (void)setupSegguestion {
    NSString *segguestText = self.pageModel.expert_suggestion;
    
    TTMShowTextView *textView = [[TTMShowTextView alloc] initWithContent:segguestText];
    
    TTMInfoView *seggustionInfoView = [[TTMInfoView alloc] initWithIconName:@"gtask_protocol_icon"
                                                                    title:@"专家建议"
                                                              contentView:textView];
    
    seggustionInfoView.origin = CGPointMake(0, self.detailInfoView.bottom + kMarginTop);
    [self.scrollView addSubview:seggustionInfoView];
    self.seggustionInfoView = seggustionInfoView;
    self.scrollView.contentSize = CGSizeMake(ScreenWidth, seggustionInfoView.bottom);
}

- (void)setupButton {
    UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
    [button setTitleColor:[UIColor whiteColor] forState:UIControlStateNormal];
    [button addTarget:self action:@selector(buttonAction:) forControlEvents:UIControlEventTouchUpInside];
    switch (self.pageModel.status) {
        case TTMApointmentStatusNotStart: {
            [button setTitle:@"开始计时" forState:UIControlStateNormal];
            [button setBackgroundImage:[UIImage resizedImageWithName:@"schedule_start_button_bg"] forState:UIControlStateNormal];
            break;
        }
        case TTMApointmentStatusStarting: {
            [button setTitle:@"结束计时" forState:UIControlStateNormal];
            [button setBackgroundImage:[UIImage resizedImageWithName:@"schedule_end_button_bg"] forState:UIControlStateNormal];
            break;
        }
        case TTMApointmentStatusWaitPay: {
            [button setTitle:@"等待付款" forState:UIControlStateNormal];
            [button setBackgroundImage:[UIImage resizedImageWithName:@"schedule_wait_button_bg"] forState:UIControlStateNormal];
            break;
        }
        case TTMApointmentStatusEnded: {
            [button setTitle:@"收费确认" forState:UIControlStateNormal];
            [button setBackgroundImage:[UIImage resizedImageWithName:@"schedule_wait_button_bg"] forState:UIControlStateNormal];
            break;
        }
        case TTMApointmentStatusComplete: {
            [button setTitle:@"已完成" forState:UIControlStateNormal];
            [button setBackgroundImage:[UIImage resizedImageWithName:@"schedule_start_button_bg"] forState:UIControlStateNormal];
            break;
        }
        default: {
            break;
        }
    }
    CGFloat buttonX = 30.f;
    CGFloat buttonW = ScreenWidth - 2 * buttonX;
    CGFloat buttonH = 40.f;
    CGFloat buttonY = self.seggustionInfoView.bottom + 3 * kMarginTop;
    
    button.frame = CGRectMake(buttonX, buttonY, buttonW, buttonH);
    
    [self.scrollView addSubview:button];
    self.button = button;
    self.scrollView.contentSize = CGSizeMake(ScreenWidth, button.bottom);
}

- (void)buttonAction:(UIButton *)button {
    TTMScheduleCellModel *model = self.pageModel;
    if (model.status == TTMApointmentStatusNotStart) { // 开始计时
        __weak __typeof(&*self) weakSelf = self;
        CYAlertView * alert = [[CYAlertView alloc] initWithTitle:@"确定要开始计时"
                                                         message:nil
                                                    clickedBlock:^(CYAlertView *alertView, BOOL cancelled, NSInteger buttonIndex) {
                                                        if (buttonIndex == 1) {
                                                            TTMApointmentModel *submitModel = [[TTMApointmentModel alloc] init];
                                                            submitModel.KeyId = model.keyId;
                                                            [weakSelf startWithModel:submitModel];
                                                        }
                                                    }
                                               cancelButtonTitle:@"取消"
                                               otherButtonTitles:@"确定", nil];
        [alert show];
    } else if (model.status == TTMApointmentStatusStarting) { // 结束计时
        __weak __typeof(&*self) weakSelf = self;
        CYAlertView * alert = [[CYAlertView alloc] initWithTitle:@"确定要结束计时"
                                                         message:nil
                                                    clickedBlock:^(CYAlertView *alertView, BOOL cancelled, NSInteger buttonIndex) {
                                                        if (buttonIndex == 1) {
                                                            TTMApointmentModel *submitModel = [[TTMApointmentModel alloc] init];
                                                            submitModel.KeyId = model.keyId;
                                                            [weakSelf endWithModel:submitModel];
                                                        }
                                                    }
                                               cancelButtonTitle:@"取消"
                                               otherButtonTitles:@"确定", nil];
        [alert show];
    } else if (model.status == TTMApointmentStatusWaitPay) { // 等待付款
        TTMChargeDetailController *chargeVC = [TTMChargeDetailController new];
        TTMApointmentModel *submitModel = [[TTMApointmentModel alloc] init];
        submitModel.KeyId = model.keyId;
        chargeVC.model = submitModel;
        [self.navigationController pushViewController:chargeVC animated:YES];
    } else if ( model.status == TTMApointmentStatusEnded) { // 收费确认
        TTMChargeConfirmController *confirmVC = [TTMChargeConfirmController new];
        TTMApointmentModel *submitModel = [[TTMApointmentModel alloc] init];
        submitModel.KeyId = model.keyId;
        confirmVC.model = submitModel;
        [self.navigationController pushViewController:confirmVC animated:YES];
    }
}


/**
 *  开始计时
 */
- (void)startWithModel:(TTMApointmentModel *)model {
    __weak __typeof(&*self) weakSelf = self;
    MBProgressHUD *hud = [MBProgressHUD showLoading];
    [TTMApointmentModel startTimeWithModel:model complete:^(id result) {
        [hud hide:YES];
        if ([result isKindOfClass:[NSString class]]) {
            [MBProgressHUD showToastWithText:result];
        } else {
            // 成功
            [weakSelf.navigationController popViewControllerAnimated:YES];
        }
    }];
}

/**
 *  结束计时
 */
- (void)endWithModel:(TTMApointmentModel *)model {
    __weak __typeof(&*self) weakSelf = self;
    MBProgressHUD *hud = [MBProgressHUD showLoading];
    [TTMApointmentModel endTimeWithModel:model complete:^(id result) {
        [hud hide:YES];
        if ([result isKindOfClass:[NSString class]]) {
            [MBProgressHUD showToastWithText:result];
        } else {
            TTMChargeConfirmController *confimVC = [TTMChargeConfirmController new];
            confimVC.model = model;
            [weakSelf.navigationController pushViewController:confimVC animated:YES];
        }
    }];
}

/**
 *  查询数据
 */
- (void)queryData {
    __weak __typeof(&*self) weakSelf = self;
    
    MBProgressHUD *hud = [MBProgressHUD showHUDWithView:self.view.window text:@"加载中..."];
    [TTMScheduleCellModel queryScheduleDetailWithId:self.model.keyId complete:^(id result) {
        [hud hide:YES];
        if ([result isKindOfClass:[NSString class]]) {
            [MBProgressHUD showToastWithText:result];
        } else {
            weakSelf.pageModel = result;
            [weakSelf setupViews];
        }
    }];
}

@end
