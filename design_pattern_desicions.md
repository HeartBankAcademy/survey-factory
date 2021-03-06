Circuit Breaker / Emergency Stop

Included circuit breaker that stops deposits in case of emergency. Only the owner can toggle on or off. Deposit function has the circuitBreaker modifier and will not allow deposits if owner fires toggleCircuitBreaker.

function toggleCircuitBreaker () 
    isOwner
    public
{
    stopped = !stopped;
}

modifier circuitBreaker {
    require(!stopped); 
    _; 
}


Restricting Access

Since the input user demographic data is not verified, addresses are approved by owner to participate. Owner adds addresses with addApprovedAddress function. Modifier isApproved prevents unapproved users from succesfully running createSurvey and answerSurvey functions.

mapping (address => bool) public approved;
modifier isApproved {
    require(approved[msg.sender] == true);
    _;
}

Only owner is allowed to toggle the circuit breaker on or off. Set owner in constructor function and use modifier to restrict access to toggleCircuitBreaker.

constructor() public {
    owner = msg.sender;
    surveyCount = 0;
}

modifier isOwner {
    require(msg.sender==owner);
    _;
}


Pull Over Push Payments

When users earn funds from surveys they are not paid out automatically but funds are added to their user balance. Users can then "Pull" by using the Withdraw function to recieve earned funds.


Off Chain Storage

Used IPFS to store survey question and answers off chain. When users create a survey the content is uploaded to IPFS and the hash of the file is saved as Surveys[].ipfsHash in the contract.
Note: IPFS connects to a remote node and will only store files for a couple hours before being dropped.


Fail Early and Fail Hard

Functions fail as early as possible by using require() at start to validate inputs and check contract state requirements.