# Status API
- '''state''' = '''pending''' | '''success''' | '''error''' | '''failure'''
- '''target_url'''
- '''description'''
- '''context'''

### Example:
'''
{
  "state": "success",
  "target_url": "https://example.com/build/status",
  "description": "The build succeeded!",
  "context": "continuous-integration/jenkins"
}
'''

# Webhook
Recieves '''POST''' requests. 

