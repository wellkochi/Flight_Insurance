pragma solidity ^0.4.26; 

contract Insurance { 
    
    // 航空公司地址 
    address public airline; 
    
    // 机上旅客总人数
    uint public totalNumber;
    
    // 赔付金额 - 假设是固定值 
    uint public amount; 
    
    // 准点时间 - 需要oracles 
    uint public desiredTime; 
    
    // 实际时间 - 需要oracles 
    uint public actualTime; 
    
    // 旅客信息
    struct Traveler {
        string name;
        address travelerAddress;
        uint number;
        bool refunded;
    }
    
    mapping(uint => Traveler) public travelers;
    
    event refundNotification(string _name, address _address, uint _amount); 
    
    
    // 航空公司专有权限 
    modifier onlyAdmin { 
        require(msg.sender == airline); 
        _; 
    } 
    
    // 检查是否晚点 
    modifier delayed { 
        require(actualTime > desiredTime); 
        _; 
    } 
    
    constructor () public { 
        // 设定航空公司为合约发起人 
        airline = msg.sender; 
        
        // 假设机上0人
        totalNumber = 0;
        
        // 假设赔付金额 = 1 eth; 
        amount = 1 ether; 
    } 
    
    // 旅客登机
    function boarding(string memory _name, address _travelerAddress) public {
        travelers[totalNumber] = Traveler({
            name: _name, 
            travelerAddress: _travelerAddress,
            number: totalNumber,
            refunded: false });
        
        totalNumber += 1;
    }
    
    // 执行赔付 
    function claim(uint _totalNumber) public payable onlyAdmin delayed { 
        
        for(uint i = 0; i < _totalNumber; i++) {
            // 检查是否已赔付过此旅客
            require(travelers[i].refunded == false);
            
            // 更新状态
            travelers[i].refunded == true;
        
            // 转账赔付 
            travelers[i].transfer(amount); 
        
            // 通知旅客 (姓名, 地址, 赔付金额) 
            emit refundNotification(travelers[i].name, travelers[i].personalAddress, amount); 
        }
        
    }
    
    // fallback 
    function() external payable {
        
    } 
}
