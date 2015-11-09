//
//  TTMScheduleDetailInfoView.m
//  ToothManager
//

#import "TTMScheduleDetailInfoView.h"
#import "TTMScheduleCellModel.h"
#import "TTMDoctorInfoLineView.h"

@interface TTMScheduleDetailInfoView ()

@property (nonatomic, strong) TTMScheduleCellModel *model;

@end

@implementation TTMScheduleDetailInfoView

- (instancetype)initWithModel:(TTMScheduleCellModel *)model {
    if (self = [super init]) {
        self.model = model;
        [self setup];
    }
    return self;
}
/**
 *  加载视图
 */
- (void)setup {
    self.backgroundColor = [UIColor whiteColor];
    
    TTMDoctorInfoLineView *timeLineView = [[TTMDoctorInfoLineView alloc] initWithTitle:@"时间"
                                                                               content:self.model.appoint_time];
    timeLineView.origin = CGPointMake(0, 0);
    [self addSubview:timeLineView];
    
    TTMDoctorInfoLineView *projectlLineView = [[TTMDoctorInfoLineView alloc] initWithTitle:@"项目"
                                                                                   content:self.model.appoint_type];
    projectlLineView.origin = CGPointMake(0, projectlLineView.bottom);
    [self addSubview:projectlLineView];
    
    NSString *duration = [[NSString stringwithNumber:@(self.model.used_time)] hourMinutesTimeFormat];
    TTMDoctorInfoLineView *durationLineView = [[TTMDoctorInfoLineView alloc] initWithTitle:@"时长"
                                                                                   content:duration];
    durationLineView.origin = CGPointMake(0, projectlLineView.bottom);
    [self addSubview:durationLineView];
    
    TTMDoctorInfoLineView *chairLineView = [[TTMDoctorInfoLineView alloc] initWithTitle:@"椅位"
                                                                                content:self.model.seat_name];
    chairLineView.origin = CGPointMake(0, durationLineView.bottom);
    [self addSubview:chairLineView];
    
    TTMDoctorInfoLineView *remarkLineView = [[TTMDoctorInfoLineView alloc] initWithTitle:@"备注"
                                                                                content:self.model.remark];
    remarkLineView.origin = CGPointMake(0, chairLineView.bottom);
    [self addSubview:remarkLineView];
    
    self.size = CGSizeMake(ScreenWidth, remarkLineView.bottom);
}

@end
