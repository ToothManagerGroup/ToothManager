//
//  TTMMessageMCell.m
//  ToothManager
//

#import "TTMMessageMCell.h"
#import "TTMMessageCellModel.h"

#define kFontSize 14.f
#define kViewH 44.f
#define kMargin 10.f

@interface TTMMessageMCell ()

@property (nonatomic, weak)   UILabel *timeLabel; // 时间
@property (nonatomic, weak)   UILabel *personLabel; // 付款人 , 椅位
@property (nonatomic, weak)   UILabel *moneyLabel; // 多少钱 , 医生名
@property (nonatomic, weak)   UILabel *stateLabel; // 收款状态 , appoint_type
@property (nonatomic, weak) UIView *separatorView; // 分隔线

@end

@implementation TTMMessageMCell

+ (instancetype)cellWithTableView:(UITableView *)tableView {
    static NSString *cellID = @"CellID";
    TTMMessageMCell *cell = [tableView dequeueReusableCellWithIdentifier:cellID];
    if (!cell) {
        cell = [[TTMMessageMCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellID];
    }
    return cell;
}

- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier {
    if (self = [super initWithStyle:style reuseIdentifier:reuseIdentifier]) {
        [self setup];
    }
    return self;
}

- (void)setup {
    
    UILabel *timeLabel = [[UILabel alloc] init];
    timeLabel.font = [UIFont systemFontOfSize:kFontSize];
    timeLabel.adjustsFontSizeToFitWidth = YES;
    [self.contentView addSubview:timeLabel];
    self.timeLabel = timeLabel;
    
    UILabel *personLabel = [[UILabel alloc] init];
    personLabel.font = [UIFont systemFontOfSize:kFontSize];
    personLabel.textAlignment = NSTextAlignmentLeft;
    [self.contentView addSubview:personLabel];
    self.personLabel = personLabel;
    
    UILabel *moneyLabel = [[UILabel alloc] init];
    moneyLabel.font = [UIFont systemFontOfSize:kFontSize];
    moneyLabel.textAlignment = NSTextAlignmentLeft;
    [self.contentView addSubview:moneyLabel];
    self.moneyLabel = moneyLabel;
    
    UILabel *stateLabel = [[UILabel alloc] init];
    stateLabel.font = [UIFont systemFontOfSize:kFontSize];
    stateLabel.textAlignment = NSTextAlignmentLeft;
    [self.contentView addSubview:stateLabel];
    self.stateLabel = stateLabel;
    
    UIView *separatorView = [[UIView alloc] init];
    separatorView.backgroundColor = TableViewCellSeparatorColor;
    separatorView.alpha = TableViewCellSeparatorAlpha;
    [self.contentView addSubview:separatorView];
    self.separatorView = separatorView;
}

- (void)setModel:(TTMMessageCellModel *)model {
    _model = model;
    if (model.isAppointment) {
        self.timeLabel.text = [[model.appoint_time dateValue] fs_stringWithFormat:@"MM-dd hh:mm"];
        self.personLabel.text = model.seat_name;
        self.moneyLabel.text = [model.doctor_name stringByAppendingString:@"医生"];
        if (model.appoint_type) {
            self.stateLabel.text = model.appoint_type;
            self.stateLabel.textColor = MainColor;
        }
    } else {
        self.timeLabel.text = [[model.datetime dateValue] fs_stringWithFormat:@"MM-dd hh:mm"];
        self.personLabel.text = [model.doctor_name stringByAppendingString:@"付款"];
        self.moneyLabel.text = [NSString stringWithFormat:@"%@元", @(model.money)];
        if (model.status == 0) {
            self.stateLabel.text = @"已收款";
            self.stateLabel.textColor = MainColor;
        } else if(model.status == 1) {
            self.stateLabel.text = @"待收款";
            self.stateLabel.textColor = [UIColor redColor];
        }
    }
    
}

- (void)layoutSubviews {
    CGFloat labelW = (ScreenWidth) / 4;
    self.timeLabel.frame = CGRectMake(kMargin, 0, labelW, kViewH);
    self.personLabel.frame = CGRectMake(labelW + kMargin, 0, labelW - kMargin, kViewH);
    self.moneyLabel.frame = CGRectMake(2 * labelW, 0, labelW, kViewH);
    self.stateLabel.frame = CGRectMake(3 * labelW, 0, labelW - 10.f, kViewH);
    self.separatorView.frame = CGRectMake(0, kViewH - TableViewCellSeparatorHeight, ScreenWidth, TableViewCellSeparatorHeight);
}

@end
