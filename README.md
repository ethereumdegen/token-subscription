 
 ## Token Subscription

 
 #### A Solidity Smartcontract facilitating the time-based subscription of users to other users and to the contract itself 
  
  * Uses 0xBTC as the subscription currency by default 
  * A default subscription price factor of 1000000 means that each 1 0xBTC paid will subscribe the user to the service for 1.15 days.     
  
  
   
 #### How does it work?


 
    function subscribeToAccount(address from, address to, uint amount) public returns (bool success)
    {
      //The 'from' address pays the 'to' address
      require(ERC20Interface(tokenCurrency).transferFrom(from,to,amount));
      
      //Additional subscription time is calculated based on the amount spent 
      uint additionalSubscriptionSeconds = amount.mul(1000).div(subscriptionPriceFactor);

      uint currentSubscriptionTime = getSubscribedUntilTime(from,to).sub(now);

      if(currentSubscriptionTime < 0)
      {
        currentSubscriptionTime = 0;
      }


      //set the new date mapping, the new subscription expiration time 
      subscribedUntil[from][to] = (now.add(currentSubscriptionTime).add( additionalSubscriptionSeconds ));

      return true;
    }


    function getSubscribedUntilTime(address from, address to) public view returns (uint time)
    { 
    
      return subscribedUntil[from][to]; //Unix timestamp

    }




 
 ----------


 
 
