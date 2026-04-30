```
// POST /v1/phones/lookup                                                                                                                                                                                           
  // Authorization: Bearer <tenant-scoped api key>
                                                                                                                                                                                                                      
  // Request                                            
  {
    "phone_e164": "+41791234567"   // strict E.164, validated server-side
  }

  // Response (status always 200; "clean" and "unknown" are first-class)
  {
    "phone_e164": "+41791234567",
    "status": "blacklist" | "greylist" | "clean" | "unknown",
    "categories": ["time_waster", "no_show", "abusive", "scammer", "dangerous"], // TODO: map to the channels' categories 
    "report_count": 12,
    "first_reported_at": "2024-09-01T12:34:00Z",
    "last_reported_at": "2025-12-15T08:11:00Z",
    "summary": "Multiple operators report ghosting after extensive negotiation.",
    "confidence": 0.85
  }
```