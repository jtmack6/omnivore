# Image Processing Service

**Date**: 2024-03-03
**Status**: Accepted
**Context**: We need to efficiently process, optimize, and serve images from saved articles, ensuring fast loading times, bandwidth optimization, and consistent image quality across different devices and contexts.

## Decision

We will implement a dedicated image processing service that provides:
- Image optimization and compression
- Multiple format support (WebP, AVIF)
- Responsive image sizes
- Caching and CDN integration
- Secure image proxying

## Consequences

### Positive

1. **Performance**
   - Faster image loading
   - Reduced bandwidth usage
   - Better user experience
   - Lower server load

2. **Quality Control**
   - Consistent image quality
   - Format optimization
   - Size optimization
   - Better compression

3. **Security**
   - Secure image proxying
   - Content validation
   - Malware scanning
   - Access control

4. **Flexibility**
   - Multiple format support
   - Responsive images
   - Custom transformations
   - Easy integration

### Negative

1. **Infrastructure**
   - Additional service to maintain
   - Storage requirements
   - CDN costs
   - Operational complexity

2. **Development Effort**
   - Service implementation
   - Integration work
   - Testing requirements
   - Documentation needs

3. **Operational Overhead**
   - Service monitoring
   - Cache management
   - Error handling
   - Performance tuning

## Alternatives

### 1. Direct Image Serving

**Pros**:
- Simple implementation
- No additional service
- Lower complexity
- Direct control

**Cons**:
- No optimization
- Higher bandwidth usage
- Poor performance
- No format conversion

### 2. Third-party CDN

**Pros**:
- Managed service
- Global distribution
- Built-in optimization
- Lower maintenance

**Cons**:
- Higher costs
- Less control
- Vendor lock-in
- Limited customization

### 3. Client-side Processing

**Pros**:
- No server overhead
- Immediate processing
- Lower server costs
- Simpler architecture

**Cons**:
- Higher client load
- Inconsistent results
- Limited capabilities
- Poor performance

## Implementation Details

1. **Service Architecture**
   ```go
   type ImageProxy struct {
     cache    Cache
     storage  Storage
     cdn      CDN
     processor Processor
   }

   func (p *ImageProxy) Process(ctx context.Context, url string, opts Options) (*Image, error) {
     // Fetch and process image
     // Apply transformations
     // Cache result
     // Return processed image
   }
   ```

2. **API Endpoints**
   ```go
   func (s *Server) routes() {
     s.router.GET("/image/*url", s.handleImage)
     s.router.GET("/image/transform/*url", s.handleTransform)
     s.router.GET("/image/info/*url", s.handleInfo)
   }
   ```

3. **Configuration**
   ```yaml
   imageproxy:
     cache:
       type: redis
       ttl: 7d
     storage:
       type: s3
       bucket: omnivore-images
     formats:
       - webp
       - avif
     sizes:
       - width: 800
       - width: 1200
       - width: 1600
   ```

4. **Integration**
   ```typescript
   interface ImageOptions {
     width?: number;
     height?: number;
     format?: 'webp' | 'avif' | 'jpeg';
     quality?: number;
   }

   function getImageUrl(url: string, options: ImageOptions): string {
     const params = new URLSearchParams(options);
     return `/image/${encodeURIComponent(url)}?${params}`;
   }
   ```

## Related ADRs

- [Docker-based Development Environment](./0003-docker-development.md)
- [Machine Learning Integration](./0007-ml-integration.md) 