//
//  TTMMessageController.m
//  ToothManager
//

#import "TTMMessageController.h"
#import "TTMSegmentedView.h"
#import "MJRefresh.h"
#import "TTMMessageCell.h"
#import "TTMMessageMCell.h"
#import "TTMIncomDetailController.h"
#import "TTMScheduleCellModel.h"

#define kSegmentH 40.f
#define kMarginTop 20.f
//#define kRowH 84.f
#define kRowH 44.f

@interface TTMMessageController ()<
    TTMSegmentedViewDelegate,
    UITableViewDelegate,
    UITableViewDataSource>

@property (nonatomic, weak)   TTMSegmentedView *segmentView;
@property (nonatomic, weak)   UITableView *tableView;
@property (nonatomic, strong) NSArray *dataArray;

@end

@implementation TTMMessageController

- (void)viewDidLoad {
    [super viewDidLoad];
    self.title = @"消息中心";
    
    [self setupSegmentView];
    [self setupTableView];
}
/**
 *  加载segment视图
 */
- (void)setupSegmentView {
    CGRect frame = CGRectMake(0, NavigationHeight + kMarginTop, ScreenWidth, kSegmentH);
    TTMSegmentedView *segmentView = [[TTMSegmentedView alloc] initWithFrame:frame];
    segmentView.segmentedTitles = @[@"未读", @"已读"];
    segmentView.delegate = self;
    [self.view addSubview:segmentView];
    self.segmentView = segmentView;
    
}
/**
 *  加载tableview
 */
- (void)setupTableView {
    CGFloat tableHeight = ScreenHeight - self.segmentView.bottom - kMarginTop;
    UITableView *tableView = [[UITableView alloc] initWithFrame:CGRectMake(0, self.segmentView.bottom + kMarginTop,
                                                                           ScreenWidth, tableHeight)
                                                          style:UITableViewStylePlain];
    tableView.delegate = self;
    tableView.dataSource = self;
    tableView.backgroundColor = [UIColor clearColor];
    tableView.separatorStyle = UITableViewCellSeparatorStyleNone;
    tableView.rowHeight = kRowH;
    
    __weak __typeof(&*self) weakSelf = self;
    [tableView addLegendHeaderWithRefreshingBlock:^{
        [weakSelf queryDataWithStatus:weakSelf.segmentView.selectedIndex];
    }];
    [self.view addSubview:tableView];
    self.tableView = tableView;
    [self.tableView.header beginRefreshing];
}

#pragma mark - UITableViewDelegate

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
    return self.dataArray.count;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    TTMMessageMCell *cell = [ TTMMessageMCell cellWithTableView:tableView];
    if (self.dataArray.count > 0) {
        TTMMessageCellModel *model = self.dataArray[indexPath.row];
        cell.model = model;
    }
    return cell;
}

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    [tableView deselectRowAtIndexPath:indexPath animated:YES];
    TTMMessageCellModel *model = self.dataArray[indexPath.row];
    if (model.isAppointment) {
        TTMIncomDetailController *detailVC = [[TTMIncomDetailController alloc] init];
        TTMScheduleCellModel *scheduleModel = [[TTMScheduleCellModel alloc] init];
        scheduleModel.appoint_id = model.appoint_id;
        detailVC.model = scheduleModel;
        [self.navigationController pushViewController:detailVC animated:YES];
    }
    
    [TTMMessageCellModel changeNotReadMessageWithId:model.keyId complete:^(id result) {
    }];
}
/**
 *  选中segment时间
 *
 *  @param segmentedView segmentedView description
 *  @param from          from description
 *  @param to            to description
 */
- (void)segmentedViewDidSelected:(TTMSegmentedView *)segmentedView fromIndex:(NSUInteger)from toIndex:(NSUInteger)to {
    [self.tableView.header beginRefreshing];
}

/**
 *  查询数据
 */
- (void)queryDataWithStatus:(TTMMessageStatusType)status {
    __weak __typeof(&*self) weakSelf = self;
    [TTMMessageCellModel queryMessageListWithStatus:status Complete:^(id result) {
        if ([result isKindOfClass:[NSString class]]) {
            [MBProgressHUD showToastWithText:result];
        } else {
            weakSelf.dataArray = result;
            [weakSelf.tableView reloadData];
        }
        [weakSelf.tableView.header endRefreshing];
    }];
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
}

@end
